# B3 Continuous Integration und Artefakte

Verantwortlich: Maximilian Noethe

## [B3-K01] Continuous Integration

### Was bedeutet Continuous Integration?
Continuous Integration (CI) ist eine **Arbeitsweise** im Team, nicht ein Werkzeug: Alle Entwicklerinnen und Entwickler führen ihre Änderungen **mindestens einmal täglich** in den gemeinsamen Hauptzweig (Mainline, z.B. `main`) zusammen. Jede Integration wird sofort durch einen **automatisierten Build mit Tests** geprüft. Ist der Build rot, hat das Reparieren höchste Priorität (oft durch Zurücknehmen des fehlerhaften Commits).

Kernpunkte:
- kleine Änderungen, häufig integriert (Stunden statt Wochen)
- ein gemeinsamer Hauptzweig, der immer lauffähig ist
- selbsttestender Build: jeder Push wird automatisch gebaut und getestet
- schnelles Feedback (Ziel: Build in rund 10 Minuten)
- ein roter Build wird sofort repariert

### Welches Problem entsteht ohne CI?
Ohne CI arbeiten alle lange getrennt und führen erst spät zusammen. Dann entsteht die sogenannte **«Integration Hell»**:
- **grosse, unberechenbare Merges**: Je länger getrennt gearbeitet wird, desto mehr Konflikte. Niemand weiss, wie lange das Zusammenführen dauert.
- **Fehler werden spät entdeckt**: Ein Fehler, der vor drei Wochen eingebaut wurde, ist schwer zu finden und teuer zu beheben.
- **semantische Konflikte**: Git kann den Text zusammenführen, aber das Programm funktioniert trotzdem nicht mehr (z.B. eine Person benennt eine Methode um, die andere ruft noch den alten Namen auf).
- **Angst vor Änderungen**: Weil Zusammenführen weh tut, wird seltener integriert und weniger refaktoriert. Der Code wird schlechter.
- Es gibt **keinen verlässlichen Stand**, von dem man jederzeit ausliefern könnte.

### Warum hat ein Team mit Pipeline, das nur alle zwei Wochen zusammenführt, noch kein CI?
Weil CI über die **Häufigkeit der Integration in den Hauptzweig** definiert ist, nicht über das Werkzeug. Eine Pipeline, die nur einen Feature-Branch baut und testet, prüft diesen Branch **isoliert**. Ob er mit der Arbeit der anderen zusammenpasst, zeigt sich erst beim Merge nach zwei Wochen, also genau das Problem, das CI lösen soll. Martin Fowler nennt so etwas einen «Continuous Build Service», aber kein Continuous Integration. Die Pipeline ist nur das Hilfsmittel, das tägliche Zusammenführen ist die Praxis.

## [B3-K02] Was eine Pipeline macht

### Was ist eine Pipeline, und wodurch wird sie ausgelöst?
Eine CI-Pipeline ist eine **automatisierte Abfolge von Schritten**, die aus dem Quellcode ein geprüftes Ergebnis (Artefakt) macht. Sie ist als Code im Repository beschrieben (z.B. `.github/workflows/*.yml` bei GitHub Actions, `.gitlab-ci.yml` bei GitLab) und läuft auf einem Server (Runner).

Auslöser (Trigger) sind typischerweise:
- ein **Push / Commit** auf einen Branch
- ein **Merge Request / Pull Request** (Prüfung vor dem Zusammenführen)
- ein **Merge in `main`** oder ein **Tag** (z.B. `v1.4.0`) für ein Release
- ein **Zeitplan** (z.B. nächtlicher Build) oder ein **manueller Start**

### Welche Schritte führt eine typische CI-Pipeline aus, und wozu dient jeder Schritt?

| Schritt | Was passiert | Wozu |
|---|---|---|
| 1. Checkout | Code der Version aus dem Repository holen | Alle bauen aus derselben, eindeutigen Quelle |
| 2. Abhängigkeiten installieren | Bibliotheken in den festgelegten Versionen laden | Reproduzierbarer Build |
| 3. Statische Prüfung (Lint) | Code-Stil und typische Fehler prüfen, ohne ihn auszuführen | Fehler und unsauberen Code früh und billig finden |
| 4. Kompilieren / Build | Code übersetzen bzw. zusammenbauen | Prüfen, ob der Code überhaupt baut |
| 5. Unit-Tests | Viele schnelle Tests einzelner Einheiten | Schnelles Feedback, ob die Logik stimmt |
| 6. Integrationstests | Zusammenspiel mit Datenbank, anderen Diensten | Fehler an Schnittstellen finden |
| 7. Artefakt erstellen | z.B. Container-Image oder JAR bauen und mit Version versehen | Ein auslieferbares, eindeutiges Paket |
| 8. Artefakt veröffentlichen | Image in die Registry hochladen | Deployment kann genau dieses Artefakt holen |

Die Schritte sind so geordnet, dass **schnelle Prüfungen zuerst** laufen. So kommt Feedback früh, und langsame Schritte laufen nur, wenn die schnellen grün sind.

### Was passiert, wenn ein Schritt fehlschlägt, und was zeichnet eine gute Pipeline aus?
Schlägt ein Schritt fehl, **bricht die Pipeline ab** («fail fast»): Die folgenden Schritte laufen nicht, es entsteht **kein Artefakt**, und der Merge Request darf nicht gemergt werden. Das Team wird benachrichtigt, und das Reparieren des roten Builds hat Vorrang vor neuer Arbeit.

Eine gute Pipeline ist:
- **schnell** (Feedback in Minuten, Richtwert rund 10 Minuten)
- **zuverlässig / reproduzierbar**: gleicher Code ergibt gleiches Ergebnis, keine zufällig fehlschlagenden («flaky») Tests
- **vollständig automatisiert**: keine manuellen Zwischenschritte
- **aussagekräftig**: klare Meldung, welcher Schritt warum fehlgeschlagen ist
- **als Code versioniert**: Änderungen an der Pipeline sind nachvollziehbar
- **sie baut ein Artefakt nur einmal** und gibt dieses weiter (siehe B3-K06)

## [B3-K03] Werkzeuge und Tests

### Wozu dienen Build-Tool, Abhängigkeitsverwaltung, Linter und Container? Nennen Sie je ein Beispiel.

| Werkzeug | Wozu | Beispiel |
|---|---|---|
| **Build-Tool** | Automatisiert Kompilieren, Testen und Verpacken mit einem einzigen Befehl, immer gleich | Maven, Gradle (Java), npm-Scripts (JavaScript) |
| **Abhängigkeitsverwaltung** | Lädt benötigte Bibliotheken in festgelegten Versionen, damit alle denselben Stand haben (Lock-Datei) | Maven (`pom.xml`), npm (`package-lock.json`), pip (`requirements.txt`) |
| **Linter** | Prüft Code statisch auf Stilregeln und typische Fehler, ohne ihn auszuführen | ESLint (JavaScript), Checkstyle (Java), Ruff/Pylint (Python) |
| **Container** | Verpackt Anwendung mit Laufzeit und Abhängigkeiten in ein Image, das überall gleich läuft («works on my machine» entfällt) | Docker (Dockerfile → Image) |

### Was zeigt die Testpyramide, und wozu dienen Unit-, Integrations- und End-to-End-Tests?
Die **Testpyramide** (ursprünglich von Mike Cohn) zeigt, **wie viele Tests welcher Art** man schreiben soll: unten viele kleine, schnelle, billige Tests, oben wenige grosse, langsame, teure Tests. Je weiter oben, desto langsamer, aufwändiger zu pflegen und anfälliger für zufällige Fehlschläge. Die umgekehrte Form («Eistüte», viele E2E-Tests, wenige Unit-Tests) gilt als Anti-Pattern.

```
        /\        End-to-End-Tests   wenige · langsam · teuer · hohes Vertrauen
       /  \
      /----\      Integrationstests  einige
     /      \
    /--------\    Unit-Tests         viele · schnell · billig
```

| Teststufe | Prüft | Wozu | Beispiel Flughafen-Service |
|---|---|---|---|
| **Unit-Test** | eine einzelne Einheit (Methode, Klasse), Abhängigkeiten werden durch Attrappen (Mocks) ersetzt | schnelles, genaues Feedback zur Logik | Die Methode zur Kapazitätsberechnung rundet korrekt |
| **Integrationstest** | Zusammenspiel mit externen Teilen: Datenbank, Dateisystem, anderer Service | Fehler an Schnittstellen finden | Ein Flughafen wird in MariaDB gespeichert und wieder gelesen |
| **End-to-End-Test** | das ganze, deployte System aus Sicht der Nutzenden | prüfen, dass wichtige Abläufe durchgängig funktionieren | Über die API einen Flughafen anlegen, dann einen Flug darauf erfassen (MS2 ruft MS1 auf) |

### Was ist ein Smoke Test?
Ein Smoke Test ist ein **kleiner, schneller und oberflächlicher Test der wichtigsten Funktionen**, meist direkt **nach einem Deployment** oder nach einem neuen Build. Er beantwortet nur die Frage: «Läuft das System grundsätzlich?» Er prüft nicht alle Details. Schlägt er fehl, ist der Build bzw. das Deployment unbrauchbar und wird zurückgewiesen, bevor weiter getestet oder freigegeben wird. Andere Namen: Build Verification Test, Confidence Test.

Beispiel: Nach dem Deployment liefert `GET /health` den Status 200, und `GET /api/airports` gibt eine Liste zurück.

## [B3-K04] Branching-Strategien

### Was ist die Grundidee von GitFlow, GitHub Flow und Trunk-based Development?

| Strategie | Grundidee | Branches | Geeignet für |
|---|---|---|---|
| **GitFlow** | Feste Struktur mit mehreren dauerhaften und kurzen Branch-Typen, Releases werden über eigene Branches vorbereitet | dauerhaft: `main` (Produktion) und `develop` (Entwicklung); dazu `feature/*`, `release/*`, `hotfix/*` | Software mit geplanten Releases und mehreren gleichzeitig unterstützten Versionen (z.B. installierte Software beim Kunden) |
| **GitHub Flow** | Ein einziger, immer auslieferbarer `main`. Jede Änderung in einem Feature-Branch, Integration über Pull Request mit Review | `main` + kurze Feature-Branches, keine Release- oder Hotfix-Branches | Webanwendungen mit einer Produktionsversion und häufigem Deployment |
| **Trunk-based Development** | Alle arbeiten direkt auf dem Hauptzweig («Trunk») oder auf sehr kurzlebigen Branches (höchstens ein bis zwei Tage). Unfertiges wird mit Feature Toggles versteckt | `main`, evtl. Branches von Stunden bis wenigen Tagen | Teams, die echtes CI und Continuous Delivery betreiben |

Wichtigster Unterschied: GitFlow arbeitet mit **langlebigen** Branches und mehreren Branch-Typen. GitHub Flow und Trunk-based Development stellen den **Hauptzweig ins Zentrum** und halten Branches **kurz**. Trunk-based Development entspricht am direktesten der Idee von CI.

### Warum sind langlebige Branches riskant?
- **Grosse, unberechenbare Merges**: Je länger ein Branch lebt, desto weiter entfernt er sich vom Hauptzweig. Das Problem ist weniger der Aufwand als die Unsicherheit: Manche Merges gehen sehr schief.
- **Semantische Konflikte**: Der Merge klappt technisch, aber das Programm ist fehlerhaft, weil zwei Änderungen inhaltlich nicht zusammenpassen.
- **Späte Entdeckung**: Konflikte und Fehler zeigen sich erst beim Zusammenführen, wenn sie schwer zu beheben sind.
- **Teufelskreis**: Schlechte Merge-Erfahrungen führen dazu, dass seltener integriert wird, was noch grössere Merges erzeugt.
- **Weniger Refactoring und weniger Sichtbarkeit**: Wer Konflikte fürchtet, räumt keinen Code auf, und das Team sieht die Arbeit der anderen nicht.
- **Kein CI**: Langlebige Branches widersprechen der täglichen Integration in den Hauptzweig.

## [B3-K05] Semantic Versioning

### Wann erhöht man MAJOR, MINOR und PATCH? Geben Sie je ein Beispiel.
Format: **MAJOR.MINOR.PATCH**, z.B. `1.3.2`. Massgebend ist die **öffentliche Schnittstelle (API)**.

| Teil | Wann erhöhen | Was wird zurückgesetzt | Beispiel (ausgehend von 1.3.2) |
|---|---|---|---|
| **MAJOR** | Änderung ist **nicht abwärtskompatibel**: bestehende Nutzer der API müssen sich anpassen | MINOR und PATCH auf 0 | Ein Feld in der JSON-Antwort wird umbenannt oder entfernt → `2.0.0` |
| **MINOR** | **neue Funktion**, die **abwärtskompatibel** ist: bestehende Nutzer merken nichts | PATCH auf 0 | Ein neuer Endpunkt kommt dazu → `1.4.0` |
| **PATCH** | **Fehlerbehebung**, abwärtskompatibel, keine neue Funktion | nichts | Ein Rechenfehler wird korrigiert → `1.3.3` |

Weitere Regeln:
- Eine veröffentlichte Version darf **nicht mehr verändert** werden. Jede Änderung braucht eine neue Versionsnummer.
- Versionen `0.x.y` gelten als Anfangsentwicklung, die API kann sich jederzeit ändern. Ab `1.0.0` ist die API stabil.
- Optional sind Vorabversionen (`2.0.0-rc.1`) und Build-Metadaten (`1.4.0+20261109`).

### Warum ist eine aussagekräftige Versionsnummer bei Microservices besonders wichtig?
- Microservices **rufen sich gegenseitig über APIs auf** (bei uns ruft MS2 den MS1 über HTTP auf). Die Versionsnummer zeigt sofort, ob eine neue Version eines Service **die anderen kaputt machen kann**: Ein MAJOR-Sprung warnt, MINOR und PATCH sind gefahrlos.
- Services werden **unabhängig voneinander** und von verschiedenen Teams deployt. Ohne klare Version weiss niemand, welche Kombinationen zusammenpassen.
- Bei einem Fehler muss man genau sagen können, **welche Version wo läuft**, und gezielt auf eine bekannte, funktionierende Version **zurückrollen** können.
- In unserem Projekt trennen sich Dev und Ops: Dev liefert ein versioniertes Image, Ops deployt genau diese Version. Die Versionsnummer ist die gemeinsame Sprache an dieser Grenze.

## [B3-K06] Artefakte und Registry

### Was ist ein Artefakt, was eine Registry? Nennen Sie Beispiele.
Ein **Artefakt** ist das **auslieferbare Ergebnis eines Builds**, also das, was am Ende deployt wird, nicht der Quellcode selbst.
Beispiele: ein Docker-Image (`ms1:1.4.0`), eine JAR-Datei (Java), ein npm-Paket, eine ZIP-Datei mit einer Webseite.

Eine **Registry** (bzw. Artefakt-Repository) ist ein **zentraler Speicher**, in dem Artefakte mit ihrer Version abgelegt werden und aus dem sie beim Deployment geholt werden.
Beispiele: Docker Hub, GitHub Container Registry (ghcr.io), GitLab Container Registry, für Pakete Maven Central, npm Registry, Nexus oder Artifactory.

### Warum werden Artefakte unveränderlich und eindeutig versioniert, und warum soll man für Deployments nicht den Tag «latest» verwenden?
**Unveränderlich und eindeutig** heisst: Eine Versionsnummer (z.B. `ms1:1.4.0`) zeigt immer auf **genau denselben Inhalt** und wird nie überschrieben. Das Artefakt wird **einmal gebaut** und dann unverändert durch alle Umgebungen (Staging → Produktion) gegeben.

Gründe:
- **Was getestet wurde, läuft auch in Produktion**: Wird für Produktion neu gebaut, könnte ein anderes Ergebnis entstehen (andere Abhängigkeiten, anderer Code).
- **Nachvollziehbarkeit**: Man weiss jederzeit, welche Version wo läuft, und kann sie bis zum Commit zurückverfolgen.
- **Rollback**: Man kann gezielt auf eine frühere, bekannte Version zurückkehren.
- **Gleichstand aller Server**: Alle Server, die `1.4.0` ausführen, haben garantiert denselben Code.

**Warum nicht `latest`?**
`latest` ist **kein fester Stand**, sondern ein **verschiebbares Etikett**, das bei jedem neuen Build auf ein anderes Image zeigt.
- Man weiss nicht, **welche Version tatsächlich läuft**.
- Verschiedene Server oder Umgebungen können zu verschiedenen Zeitpunkten **unterschiedliche Images** unter demselben Namen holen.
- **Rollback ist kaum möglich**: «Zurück auf latest» zeigt wieder auf das fehlerhafte Image.
- Ein Neustart kann **unbemerkt eine neue Version** einspielen.
Die Kubernetes-Dokumentation rät deshalb ausdrücklich davon ab, `latest` in Produktion zu verwenden. Stattdessen deployt man eine feste Version (z.B. `1.4.0`) oder den Commit-Hash bzw. den Digest des Images.

## [B3-K07] Fall Knight Capital und Versionierung

### Nennen Sie drei Ursachen des Vorfalls, die mit diesem Block zu tun haben.
1. **Manuelles, nicht automatisiertes Deployment**: Ein Techniker kopierte die neue Software von Hand auf acht Server. Es gab kein automatisiertes, wiederholbares Verfahren, das garantiert, dass alle Server dasselbe erhalten. Ein Server wurde vergessen.
2. **Kein eindeutig versioniertes Artefakt und keine Kontrolle**: Niemand konnte prüfen, welche Version auf welchem Server lief. Es gab weder eine schriftliche Anleitung noch eine zweite Person oder eine automatische Prüfung (z.B. Smoke Test, Versionsabgleich), die bemerkt hätte, dass ein Server noch die alte Version hatte.
3. **Toter Code im System**: Die alte Funktion «Power Peg» wurde seit Jahren nicht mehr gebraucht, aber nie entfernt. Sie war weiterhin im Code und konnte aufgerufen werden.
4. **Wiederverwendetes Konfigurations-Flag**: Die neue Software verwendete ein Flag mit neuer Bedeutung, das früher Power Peg einschaltete. Auf dem alten Server bedeutete dasselbe Flag deshalb etwas völlig anderes.

### Was hätte ein versioniertes Artefakt mit automatisiertem Deployment verändert?
- Die neue Software wäre **einmal** als Artefakt mit eindeutiger Version (z.B. `2.0.0`) gebaut und in eine Registry gelegt worden.
- Ein **automatisiertes Deployment** hätte **dieselbe Version auf alle acht Server** verteilt. Das Vergessen eines Servers durch einen Menschen wäre so nicht passiert.
- Das Deployment hätte **geprüft**, ob überall die erwartete Version läuft (z.B. Versionsabfrage oder Smoke Test pro Server), und bei einer Abweichung **abgebrochen**, bevor das Flag eingeschaltet wurde.
- Bei einem Problem wäre ein **schneller, sauberer Rollback** auf die vorherige bekannte Version für alle Server möglich gewesen.
- Man hätte jederzeit **nachvollziehen** können, welche Version wo läuft. Die Fehlersuche hätte nicht so lange gedauert.

### Was lernen Sie aus dem Fall über alten Code und wiederverwendete Flags?
- **Toten Code entfernen**: Code, der nicht mehr gebraucht wird, gehört gelöscht und nicht nur abgeschaltet. Solange er im Programm ist, kann er wieder aktiviert werden, und er wird nicht mehr getestet.
- **Flags nie mit neuer Bedeutung wiederverwenden**: Ein neues Feature bekommt ein **neues Flag** mit eigenem Namen. Sonst bedeutet dasselbe Flag in alter und neuer Version etwas Verschiedenes, und eine gemischte Umgebung wird gefährlich.
- **Feature Toggles aufräumen**: Flags sind nur vorübergehend. Ist ein Feature fertig, wird das Flag samt altem Code entfernt.
- **Alte und neue Version gleichzeitig bedenken**: Bei einem Deployment können kurz beide Versionen laufen. Eine Konfigurationsänderung muss mit beiden sicher sein.

### Vergeben Sie die vier Versionsnummern aus Teil B.
Ausgangsversion: **1.3.2**. Jede Änderung baut auf der vorherigen auf.

| Nr. | Änderung | Art der Änderung | Neue Version |
|---|---|---|---|
| 1 | Ein Rundungsfehler bei der Kapazität wird korrigiert | Fehlerbehebung, kompatibel → **PATCH** | **1.3.3** |
| 2 | Neuer Endpunkt `GET /api/airports/{id}` | neue Funktion, kompatibel (Bestehendes funktioniert weiter) → **MINOR**, PATCH auf 0 | **1.4.0** |
| 3 | Das Feld `code` heisst neu `iataCode` | **nicht abwärtskompatibel**: Clients, die `code` lesen oder senden, funktionieren nicht mehr → **MAJOR**, MINOR und PATCH auf 0 | **2.0.0** |
| 4 | Fehlermeldung bei doppeltem Kürzel wird verständlicher formuliert | Verbesserung ohne neue Funktion und ohne API-Bruch (Statuscode und Struktur bleiben gleich) → **PATCH** | **2.0.1** |

Hinweis zu Nr. 4: Das gilt, solange nur der Text der Meldung ändert. Würde sich der HTTP-Statuscode oder die Struktur der Fehlerantwort ändern, auf die sich Clients verlassen, wäre es eine nicht kompatible Änderung.

## Quellen
- Martin Fowler: *Continuous Integration* (2024). https://martinfowler.com/articles/continuousIntegration.html
- Ham Vocke (auf martinfowler.com): *The Practical Test Pyramid*. https://martinfowler.com/articles/practical-test-pyramid.html
- Martin Fowler: *Patterns for Managing Source Code Branches*. https://martinfowler.com/articles/branching-patterns.html
- Tom Preston-Werner: *Semantic Versioning 2.0.0* (deutsch). https://semver.org/lang/de/
- Kubernetes-Dokumentation: *Images*, Hinweis zum Tag `latest`. https://kubernetes.io/docs/concepts/containers/images/
- TechTarget: *What is smoke testing?* https://www.techtarget.com/searchsoftwarequality/definition/smoke-testing
- U.S. Securities and Exchange Commission: Presse-Zusammenfassung zum Verfahren gegen Knight Capital (16.10.2013). https://www.sec.gov/news/digest/2013/dig101613.htm
- Falltext Knight Capital aus dem Auftrag B3 (Modul 324)
- KI-Werkzeug: Claude (Anthropic), zum Recherchieren, Strukturieren und Formulieren. Aussagen wurden mit den oben genannten Quellen abgeglichen.
