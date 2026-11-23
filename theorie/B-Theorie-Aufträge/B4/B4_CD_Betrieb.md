# B4 Continuous Delivery, Umgebungen und Deployment

## [B4-K01] Continuous Delivery und Continuous Deployment

### 1. Was ist der Unterschied zwischen Continuous Delivery und Continuous Deployment?

Beide bauen auf Continuous Integration auf: Jede Änderung läuft automatisch durch Build, Tests und Pipeline. Der Unterschied liegt im **letzten Schritt**, dem Weg in die Produktion.

| | Continuous Delivery | Continuous Deployment |
| --- | --- | --- |
| Stand nach der Pipeline | Jede grüne Änderung ist **jederzeit bereit** für die Produktion | Jede grüne Änderung geht **automatisch live** |
| Weg in die Produktion | Ein Mensch entscheidet und löst das Deployment aus («Knopfdruck») | Kein manueller Freigabeschritt, die Pipeline deployt selbst |
| Wer bestimmt den Zeitpunkt? | Das Team bzw. das Business | Die Pipeline (sobald alle Prüfungen grün sind) |
| Typisch für | Software mit festen Release-Terminen, regulierte Umfelder (z.B. Banken) | Webdienste mit vielen Deployments pro Tag (z.B. Amazon, Netflix) |

Kurz: Bei Continuous Delivery **könnte** man jederzeit ausliefern, bei Continuous Deployment **wird** jede Änderung ausgeliefert. Martin Fowler beschreibt Continuous Delivery so, dass die Software während ihres ganzen Lebens auslieferbar bleibt und Deployment-Bereitschaft Vorrang vor neuen Features hat.

### 2. Was muss ein Team beherrschen, bevor es Continuous Deployment einführen kann, und warum?

Bei Continuous Deployment prüft **niemand mehr von Hand**, bevor eine Änderung live geht. Die Pipeline und das Monitoring übernehmen die Rolle der Qualitätskontrolle. Deshalb braucht das Team:

- **Zuverlässige automatische Tests**: Sie sind die einzige Prüfung vor der Produktion. Lücken oder zufällig fehlschlagende («flaky») Tests führen direkt zu Fehlern beim Kunden.
- **Eine stabile Pipeline**: Sie muss bei jedem Lauf gleich und nachvollziehbar arbeiten, sonst ist das Ergebnis nicht vertrauenswürdig.
- **Monitoring und Alarme**: Fehler, die trotz Tests durchrutschen, müssen in Minuten auffallen und nicht erst durch Kundenbeschwerden.
- **Schnellen Rollback bzw. Roll-forward**: Ein fehlerhaftes Release muss sofort zurückgenommen oder korrigiert werden können.
- **Kleine Änderungen**: Kleine Commits sind leicht zu prüfen und bei einem Fehler schnell zu finden.
- **Sichere Deployment-Strategien**: Canary Releases oder Feature Toggles begrenzen den Schaden eines Fehlers (siehe B4-K05).

Warum? Ohne diese Grundlagen automatisiert Continuous Deployment nur die Auslieferung von Fehlern: Jeder Bug erreicht ungebremst alle Nutzenden.

## [B4-K02] Umgebungen und «Build once, deploy many»

### 1. Wozu dienen die Umgebungen Dev, Test, Staging und Produktion?

Eine Änderung durchläuft mehrere Umgebungen. Jede Stufe prüft etwas anderes und ist näher an der echten Produktion als die vorherige.

| Umgebung | Zweck | Wer nutzt sie |
| --- | --- | --- |
| **Dev** (Development) | Entwickeln und erste Integration; darf instabil sein | Entwicklerinnen und Entwickler |
| **Test** | Automatische und manuelle Tests in einer isolierten Umgebung | Pipeline, Testende |
| **Staging** | Generalprobe: so nahe wie möglich an der Produktion, letzte Prüfung vor dem Release | Team, Fachabteilung (Abnahme) |
| **Produktion** | Das Live-System mit echten Daten | Echte Nutzerinnen und Nutzer |

Ziel: Fehler sollen möglichst früh und in einer Umgebung auftreten, in der sie keinen echten Schaden anrichten.

### 2. Was ist Umgebungs-Parität, und warum ist sie wichtig?

**Umgebungs-Parität** heisst: Alle Umgebungen sind so gleich wie möglich aufgebaut. Gleiches Betriebssystem, gleiche Versionen von Laufzeit und Datenbank, gleiche Dienste. Die Twelve-Factor App nennt das «Dev/Prod-Parität».

Beispiel: Dev, Test, Staging und Produktion laufen alle auf Linux mit Node 20 und PostgreSQL.

Warum wichtig?

- Ein Test sagt nur etwas aus, wenn er unter **denselben Bedingungen** läuft wie später die Produktion.
- Weicht eine Umgebung ab, zeigt sich ein Fehler erst in der Produktion. Beispiel: Test läuft mit SQLite, Produktion mit PostgreSQL. Eine Abfrage funktioniert im Test, scheitert aber in der Produktion.
- Ohne Parität entsteht das bekannte «Bei mir hat es funktioniert»-Problem, nur eine Stufe später («In Staging lief es doch»).

### 3. Was bedeutet «Build once, deploy many», und warum ist das wichtig?

**«Build once, deploy many»** heisst: Das Artefakt (z.B. ein Docker-Image `app:1.4.0`) wird **genau einmal** gebaut. Danach geht **dasselbe Artefakt** unverändert durch alle Umgebungen: Test → Staging → Produktion. Pro Umgebung ändert sich nur die **Konfiguration** (siehe B4-K03), nie der Code.

Warum wichtig?

- **In der Produktion läuft genau das, was getestet wurde.** Würde man pro Umgebung neu bauen, könnten andere Abhängigkeitsversionen oder ein anderer Code-Stand hineinrutschen. Dann hat man etwas ausgeliefert, das nie getestet wurde.
- **Nachvollziehbarkeit**: Die Version (bzw. der Commit-Hash) ist überall dieselbe. Man weiss genau, was wo läuft.
- **Schneller und günstiger**: Nur ein Build statt einen pro Umgebung.
- **Einfacher Rollback**: Die vorherige Version liegt als fertiges Artefakt in der Registry bereit.

## [B4-K03] Infrastructure as Code und Konfiguration

### 1. Was ist Infrastructure as Code, und welchen Vorteil hat es gegenüber von Hand eingerichteten Servern?

**Infrastructure as Code (IaC)** bedeutet: Server, Netzwerke, Datenbanken und andere Infrastruktur werden in **Dateien im Repository** beschrieben statt von Hand eingerichtet. Ein Werkzeug liest diese Dateien und erstellt die Infrastruktur automatisch.

Beispiele für Werkzeuge: Terraform, Bicep (Azure), Ansible, Kubernetes-Manifeste oder `docker-compose.yml`.

| | Von Hand eingerichtet | Infrastructure as Code |
| --- | --- | --- |
| **Wiederholbar** | Jeder Server ist ein Einzelstück («Snowflake-Server»), niemand weiss genau, wie er entstanden ist | Jede Umgebung entsteht aus denselben Dateien gleich, beliebig oft |
| **Versioniert** | Änderungen sind nicht nachvollziehbar | Jede Änderung ist ein Commit mit Autor, Datum und Begründung |
| **Überprüfbar** | Kein Review möglich | Änderungen laufen über Merge Request und Review wie normaler Code |
| **Parität** | Umgebungen driften mit der Zeit auseinander (Configuration Drift) | Alle Umgebungen aus derselben Vorlage, Unterschiede sind sichtbar |
| **Wiederherstellung** | Neuaufbau nach einem Ausfall dauert Tage | Neuaufbau mit einem Befehl (z.B. `terraform apply`) |

Beispiel: Ein Team braucht eine neue Staging-Umgebung. Ohne IaC ist das ein Ticket an den Betrieb und dauert Tage. Mit IaC wird dieselbe Vorlage wie für die Produktion verwendet, und die Umgebung steht in Minuten.

### 2. Warum wird Konfiguration (z.B. Datenbankadresse) von aussen übergeben statt im Code festgelegt?

**Konfiguration** ist alles, was sich zwischen den Umgebungen unterscheidet: Datenbankadresse, URLs anderer Dienste, Log-Level, Zugangsdaten. Sie wird **von aussen** übergeben, typischerweise über **Umgebungsvariablen** (Twelve-Factor App, Faktor III).

Beispiel: Dasselbe Image `v1.4` bekommt im Test `DB_HOST=test-db.intern` und in der Produktion `DB_HOST=prod-db.intern`.

Gründe:

- **Voraussetzung für «Build once, deploy many»**: Ein Artefakt läuft so in jeder Umgebung, ohne neu gebaut zu werden. Wäre die Datenbankadresse im Code, bräuchte jede Umgebung einen eigenen Build.
- **Keine Secrets im Code**: Passwörter landen nicht im Repository (siehe B4-K04).
- **Ändern ohne Neu-Deployment des Codes**: Eine neue Datenbankadresse braucht nur eine neue Konfiguration, keinen neuen Build.

Die Twelve-Factor App gibt als Test: Der Code könnte jederzeit öffentlich gemacht werden, ohne dass Zugangsdaten preisgegeben werden.

In .NET ist das direkt eingebaut: `IConfiguration` liest `appsettings.json`, und Umgebungsvariablen überschreiben diese Werte pro Umgebung.

## [B4-K04] Secrets

### 1. Was sind Secrets, und wo gehören sie hin bzw. nicht hin (Repository, Container-Image)?

**Secrets** sind vertrauliche Zugangsdaten, mit denen man auf Systeme oder Daten zugreifen kann: Passwörter, API-Schlüssel, Tokens, Zertifikate und private Schlüssel, Connection-Strings mit Passwort.

| Ort | Geeignet? | Begründung |
| --- | --- | --- |
| Git-Repository (Code, `appsettings.json`, `.env`) | **Nein** | Jede Person mit Lesezugriff sieht das Secret, und die Git-Historie vergisst nichts |
| Container-Image (z.B. im Dockerfile) | **Nein** | Images werden geteilt und in Registries gespeichert; jede Schicht lässt sich auslesen |
| Logs, Tickets, Chat | **Nein** | Werden breit geteilt und lange aufbewahrt |
| **Secret-Store** (z.B. Azure Key Vault, HashiCorp Vault, Kubernetes Secrets) | **Ja** | Zentral, verschlüsselt, mit Zugriffsrechten und Protokoll |
| Geschützte Variablen der CI/CD-Plattform (GitLab CI/CD Variables, GitHub Secrets) | **Ja** | Werden in Logs maskiert und nur der Pipeline übergeben |

Grundregel: Secrets werden **erst beim Start** der Anwendung von aussen übergeben, z.B. als Umgebungsvariable oder aus dem Secret-Store gelesen. Das Image und der Code enthalten nur den **Namen** des Secrets, nie den Wert.

### 2. Warum reicht es nicht, ein versehentlich committetes Passwort wieder aus dem Repository zu löschen?

- **Die Git-Historie vergisst nicht**: Ein neuer Commit, der das Passwort entfernt, löscht es nur aus dem aktuellen Stand. In den früheren Commits ist es weiterhin vorhanden und mit `git log` abrufbar.
- **Kopien existieren bereits**: Jede Person, die das Repository geklont oder geforkt hat, besitzt das Passwort lokal. Auch CI-Caches und Backups können es enthalten.
- **Bots suchen aktiv danach**: Bei öffentlichen Repositories scannen automatisierte Programme GitHub und GitLab nach Schlüsseln, oft innert Minuten nach dem Push.

Deshalb gilt: Ein committetes Secret ist **kompromittiert** und muss **sofort geändert** werden (Rotation). Das Bereinigen der Historie (z.B. mit `git filter-repo`) ist nur ein zweiter, ergänzender Schritt.

### 3. Was bedeuten Rotation und Least Privilege?

**Rotation**: Secrets werden **regelmässig** und **nach jedem Leck** durch neue ersetzt. Ein gestohlenes Secret ist dadurch nur für begrenzte Zeit gültig. Beispiel: Ein API-Schlüssel wird alle 90 Tage erneuert, und das alte Passwort wird ungültig. Idealerweise läuft die Rotation automatisch, damit sie nicht vergessen wird.

**Least Privilege** (Prinzip der minimalen Rechte): Jeder Dienst und jede Person bekommt **nur die Rechte, die wirklich nötig** sind. Wird ein Secret gestohlen, ist der mögliche Schaden klein.

Beispiel: Ein Reporting-Dienst liest nur Daten. Er erhält deshalb einen Datenbank-Benutzer mit Leserechten, keine Rechte zum Löschen und keine Admin-Rechte. Gelangt sein Passwort in falsche Hände, kann der Angreifer Daten lesen, aber nichts löschen oder verändern.

## [B4-K05] Deployment-Strategien und Feature Toggles

### 1. Wie funktionieren Recreate, Rolling, Blue/Green und Canary? Nennen Sie je einen Vor- und Nachteil.

| Strategie | Funktionsweise | Vorteil | Nachteil |
| --- | --- | --- | --- |
| **Recreate** | Alle Instanzen der alten Version werden gestoppt, danach wird die neue Version gestartet | Einfach; es läuft nie mehr als eine Version gleichzeitig | **Ausfallzeit** (Downtime) zwischen Stopp und Start |
| **Rolling** | Die Instanzen werden **eine nach der anderen** durch die neue Version ersetzt | Keine Ausfallzeit, keine zusätzliche Infrastruktur nötig | Alte und neue Version laufen eine Zeit lang **parallel**; Rollback dauert ebenso lange wie das Deployment |
| **Blue/Green** | Die neue Version (Green) läuft **komplett neben** der alten (Blue). Ist sie geprüft, wird der gesamte Verkehr mit einem Schalter (Load Balancer, Router) auf Green umgestellt | **Blitzschneller Rollback**: einfach zurück auf Blue schalten; Green kann vor dem Umschalten getestet werden | **Doppelte Infrastruktur**; die Datenbank wird meist geteilt, Schemaänderungen bleiben schwierig |
| **Canary** | Zuerst bekommt nur ein **kleiner Teil der Nutzer** (z.B. 5 %) die neue Version. Läuft alles gut, wird der Anteil schrittweise erhöht | Ein Fehler trifft **nur wenige**; Test mit echten Nutzern unter echter Last | Braucht **gutes Monitoring**, um Probleme der kleinen Gruppe zu erkennen; komplexeres Routing |

Der Name «Canary» kommt vom Kanarienvogel im Bergbau: Er zeigte giftige Gase an, bevor die Bergleute gefährdet waren. So zeigt die kleine Nutzergruppe Fehler an, bevor alle betroffen sind.

### 2. Was ist ein Feature Toggle, und was ist mit «Deployment und Release trennen» gemeint?

Ein **Feature Toggle** (auch Feature Flag) ist ein **Schalter im Code**, der eine Funktion zur Laufzeit ein- oder ausschaltet, ohne dass neuer Code deployt werden muss.

```csharp
if (await featureManager.IsEnabledAsync("NeuerCheckout"))
    return NeuerCheckout();
return AlterCheckout();
```

(In .NET z.B. mit der Bibliothek `Microsoft.FeatureManagement`.)

**Deployment und Release trennen** bedeutet:

- **Deployment** = Technik: Der neue Code wird auf die Server ausgeliefert. Er ist da, aber noch ausgeschaltet.
- **Release** = Entscheidung: Die Funktion wird für Nutzerinnen und Nutzer freigeschaltet, indem der Toggle eingeschaltet wird.

Dadurch kann ein Team:

- **jederzeit deployen**, auch wenn eine Funktion noch nicht fertig ist (passt zu Trunk-based Development),
- den **Zeitpunkt des Releases** unabhängig bestimmen, z.B. passend zu einer Marketingkampagne,
- eine Funktion **nur für bestimmte Gruppen** freischalten (interne Mitarbeitende, Beta-Testende),
- eine fehlerhafte Funktion **sofort ausschalten**, ohne Rollback.

Nachteil: Toggles sind temporär gedacht. Werden sie nicht entfernt, entsteht toter Code und ein schwer überblickbares Geflecht von Schaltern (vgl. Fall Knight Capital in B3).

## [B4-K06] Rollback und Datenbankänderungen

### 1. Was ist der Unterschied zwischen Rollback und Roll-forward?

| | Rollback | Roll-forward |
| --- | --- | --- |
| Vorgehen | Zurück zur **letzten funktionierenden Version** (z.B. v2 fehlerhaft → zurück auf v1) | Den Fehler **schnell beheben** und eine **neue Version** ausliefern (v2 fehlerhaft → v3 mit Fix) |
| Geschwindigkeit | Meist sehr schnell, da die alte Version als Artefakt bereitliegt | Hängt davon ab, wie schnell der Fix gefunden und durch die Pipeline gebracht wird |
| Sinnvoll, wenn | Die Ursache unklar ist und der Dienst sofort wieder laufen muss | Ein Rollback nicht möglich ist (z.B. nach Datenbankänderungen) oder der Fix klein und offensichtlich ist |
| Voraussetzung | Versionierte Artefakte, «Build once, deploy many» | Schnelle, zuverlässige Pipeline |

Im Incident gilt: Zuerst den Dienst wiederherstellen. Ist ein Rollback möglich, ist er oft der schnellste Weg dazu.

### 2. Warum ist ein Rollback schwierig, wenn eine neue Version das Datenbankschema verändert hat?

Code lässt sich austauschen, **Daten nicht**. Das Artefakt der alten Version liegt unverändert in der Registry, die Datenbank aber hat sich weiterentwickelt.

Beispiel: Version 2 benennt die Spalte `name` in `vollname` um. Nach einem Rollback auf Version 1 sucht der alte Code die Spalte `name` und findet sie nicht mehr. Die Anwendung stürzt ab.

Weitere Probleme:

- **Daten, die seit dem Deployment geschrieben wurden**, liegen im neuen Format vor. Ein Rückgängigmachen des Schemas kann sie beschädigen oder verlieren.
- **Gelöschte Spalten oder Tabellen** sind weg. Ein Down-Skript kann die Struktur wiederherstellen, aber nicht die Daten.
- Bei **Rolling- und Blue/Green-Deployments** laufen alte und neue Version gleichzeitig gegen dieselbe Datenbank. Das Schema muss also für beide Versionen passen.

### 3. Wie funktioniert das Prinzip «Expand and Contract»?

**Expand and Contract** (auch «Parallel Change», beschrieben von Danilo Sato auf martinfowler.com) zerlegt eine nicht kompatible Schemaänderung in mehrere kleine, **kompatible** Schritte. Nach jedem Schritt funktionieren die alte und die neue Version des Codes.

Beispiel: Die Spalte `name` soll durch `vollname` ersetzt werden.

1. **Expand (Erweitern)**: Die neue Spalte `vollname` kommt dazu, die alte Spalte `name` bleibt bestehen. Der Code schreibt in **beide** Spalten.
2. **Migrate (Umstellen)**: Bestehende Daten werden von `name` nach `vollname` kopiert. Danach liest der Code nur noch aus `vollname`.
3. **Contract (Zusammenziehen)**: Erst wenn **keine Version** mehr die alte Spalte braucht, wird `name` in einem späteren Release entfernt.

Vorteile:

- In jeder Phase bleibt ein **Rollback möglich**, weil die alte Spalte noch existiert.
- Alte und neue Version können **gleichzeitig laufen**. Das macht Rolling- und Blue/Green-Deployments mit einer gemeinsamen Datenbank erst möglich.

Nachteil: Die Änderung braucht mehrere Releases und für eine Zeit doppelte Logik im Code.

Merksatz: **Neues hinzufügen, Daten umziehen, Altes entfernen, nie alles in einem Schritt.**

## [B4-K07] Fall CrowdStrike

Am 19. Juli 2024 verteilte CrowdStrike ein fehlerhaftes Inhalts-Update (Erkennungsregeln) **gleichzeitig an alle** Kunden. Ein automatischer Prüfschritt (Validator) hatte den Fehler wegen eines eigenen Fehlers nicht erkannt. Rund **8,5 Millionen** Windows-Rechner stürzten ab und starteten nicht mehr, u.a. bei Fluggesellschaften, Spitälern und Banken.

### 1. Welche Deployment-Strategie hätte die Auswirkung am stärksten begrenzt?

Ein **gestaffelter Rollout im Sinne von Canary**:

1. Zuerst **intern** auf den eigenen Rechnern von CrowdStrike.
2. Dann an eine **kleine Kundengruppe** (z.B. 1 %).
3. Erst wenn das Monitoring keine Probleme zeigt, schrittweise an **alle**.

Begründung: Der Fehler war nicht subtil. Die Rechner stürzten sofort ab und starteten nicht mehr. Ein Canary hätte das in Minuten auf wenigen Geräten gezeigt. Der Rollout hätte gestoppt werden können, bevor Millionen von Rechnern betroffen waren. Die Strategie begrenzt den **Schadensradius** (Blast Radius): Statt 8,5 Millionen Rechner wären nur wenige betroffen gewesen.

Blue/Green passt hier nicht, weil es sich nicht um einen Server-Dienst handelt, sondern um Software auf Millionen einzelner Kundengeräte. Rolling allein hilft wenig, wenn niemand das Ergebnis überwacht und den Rollout stoppt.

### 2. Warum war ein Rollback besonders schwierig?

- **Die Rechner starteten nicht mehr.** Ein normaler Rollback setzt voraus, dass das System erreichbar ist und ein Update empfangen kann. Hier stürzte Windows beim Start ab, bevor die CrowdStrike-Software eine Korrektur herunterladen konnte. Der Fehler hat also den **Weg zur Behebung selbst blockiert**.
- **Reparatur von Hand, Gerät für Gerät**: Techniker mussten jeden Rechner einzeln im abgesicherten Modus starten und die fehlerhafte Datei löschen. Bei verschlüsselten Festplatten (BitLocker) brauchte es zusätzlich für jedes Gerät den Wiederherstellungsschlüssel.
- **Alle gleichzeitig betroffen**: Es gab keine gesunde Mehrheit, die weiterarbeiten konnte, während repariert wurde. IT-Abteilungen mussten Tausende Geräte auf einmal reparieren. Das dauerte Tage.

### 3. Was zeigt der Fall über das Verhältnis von automatischen Tests und gestaffeltem Rollout?

- **Tests allein reichen nicht.** Auch automatische Tests können Fehler haben. Hier hatte der Validator selbst einen Fehler und liess das fehlerhafte Update durch.
- **Der gestaffelte Rollout ist das zweite Sicherheitsnetz.** Er prüft unter echten Bedingungen, was die Tests nicht erkannt haben, und begrenzt den Schaden, wenn ein Fehler durchrutscht.
- **Es braucht beides.** Tests finden Fehler früh und günstig. Der gestaffelte Rollout ersetzt die Tests nicht, sondern fängt das Restrisiko ab. Er funktioniert nur zusammen mit gutem Monitoring, das einen Fehler in der ersten Gruppe auch erkennt.

Allgemein: Je mehr Systeme eine Änderung erreicht und je schwerer sie rückgängig zu machen ist, desto wichtiger ist ein gestaffelter Rollout.

### 4. Sicherheits-Updates sofort an alle? Nennen Sie je ein Pro und ein Kontra.

| | Argument |
| --- | --- |
| **Pro** | Neue Bedrohungen (z.B. Zero-Day-Angriffe) verbreiten sich schnell. Jede Stunde Verzögerung lässt Kunden ungeschützt. Ein Update, das sofort überall ankommt, wehrt den Angriff überall gleichzeitig ab. |
| **Kontra** | Ein Fehler trifft ebenfalls **alle gleichzeitig**. Bei CrowdStrike richtete das fehlerhafte Update weit mehr Schaden an als die meisten Angriffe, gegen die es schützen sollte. |

Einschätzung: Ein gestaffelter Rollout über wenige Stunden ist ein vertretbarer Kompromiss. Der Schutz kommt nur geringfügig später, und ein Fehler bleibt auf eine kleine Gruppe begrenzt. CrowdStrike kündigte nach dem Vorfall genau das an: Inhalts-Updates gestaffelt ausliefern (zuerst intern, dann an kleine Kundengruppen), das Monitoring während des Rollouts verbessern und Kunden mehr Kontrolle über den Zeitpunkt geben.

## Quellen

- Martin Fowler: *Continuous Delivery* (2013, ergänzt 2014). https://martinfowler.com/bliki/ContinuousDelivery.html
- Adam Wiggins: *The Twelve-Factor App, III. Konfiguration* (deutsch). https://12factor.net/de/config
- Danilo Sato (auf martinfowler.com): *Canary Release* (2014). https://martinfowler.com/bliki/CanaryRelease.html
- Danilo Sato (auf martinfowler.com): *Parallel Change* (Expand and Contract, 2014). https://martinfowler.com/bliki/ParallelChange.html
- Falltext CrowdStrike (19. Juli 2024) aus dem Auftrag B4 (Modul 324)
- TBZ Modul 324: Lernvideo B4 «Continuous Delivery, Umgebungen und Deployment»
- KI-Werkzeug: Claude (Anthropic), zum Strukturieren und Formulieren. Aussagen wurden mit den oben genannten Quellen abgeglichen.
