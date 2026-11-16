# B1 DevOps-Kultur und Umgang mit Störungen

Dieses Dokument umfasst die theoretische Ausarbeitung des Blocks B1 (DevOps-Kultur und Umgang mit Störungen) gemäss Kriterienkatalog der TBZ (Technische Berufsschule Zürich). Es beantwortet sämtliche Leitfragen zu den Kriterien B1-K01 bis B1-K07 in der geforderten Tiefe.

## [B1-K01] DevOps-Begriff, Zielkonflikt und Missverständnisse

### 1. Was versteht man unter DevOps? Ist DevOps ein Werkzeug, eine Rolle oder etwas anderes?

DevOps ist weder ein einzelnes Softwarewerkzeug noch eine isolierte Berufsbezeichnung oder Zertifizierung. Vielmehr ist DevOps eine Kultur-, Denk- und Arbeitsphilosophie, die darauf abzielt, die historisch getrennten Bereiche Softwareentwicklung (Development / Dev) und IT-Betrieb (Operations / Ops) über den gesamten Lebenszyklus eines Softwaresystems hinweg zusammenzuführen.

**Kernpunkte der DevOps-Definition:**

Shared Responsibility (Geteilte Verantwortung): Entwickler und Betriebler tragen gemeinsam die Verantwortung für Konzeption, Build, Test, Bereitstellung, Betrieb, Skalierung und Incident-Behebung. Das berühmte Zitat von Werner Vogels (CTO Amazon) fasst dies prägnant zusammen: «You build it, you run it».
Ganzheitlicher Lebenszyklus: Qualität, Sicherheit und Wartbarkeit sind nicht nachgelagerte Schritte, sondern integraler Bestandteil jedes Entwicklungsschritts.
Kollaboration & Empathie: Der Fokus liegt auf Vertrauen, transparenter Kommunikation und dem Abbau von Silos, unterstützt durch Automatisierung und standardisierte Prozesse.

| Dimension | Was DevOps nicht ist | Was DevOps wirklich ist |
| --- | --- | --- |
| Werkzeug | Reine Tool-Sammlung (z.B. nur Docker, Jenkins oder Kubernetes) | Eine Philosophie, bei der Werkzeuge Kultur und Automatisierung unterstützen |
| Rolle / Team | Ein einzelner «DevOps-Engineer» oder ein neues isoliertes «DevOps-Team» | Organisationsweite Zusammenarbeit; Dev und Ops arbeiten hand in hand |
| Ziel | Nur schnelle Releases auf Kosten der Stabilität | Hohe Änderungsgeschwindigkeit bei gleichzeitiger hoher Stabilität und Ausfallsicherheit |

### 2. Welcher Zielkonflikt besteht klassisch zwischen Entwicklung und Betrieb, und was ist mit «Wall of Confusion» gemeint?

Der klassische Zielkonflikt

Traditionelle IT-Organisationen steuern Softwareentwicklung und IT-Betrieb über gegensätzliche Anreizsysteme und Kennzahlen:

Development (Dev): Wird an Geschwindigkeit und Funktionszuwachs gemessen (Speed of Change, Time-to-Market, Anzahl neuer Features). Entwickler werden dafür belohnt, möglichst schnell und häufig neuen Code zu produzieren.
Operations (Ops): Wird an Stabilität, Verfügbarkeit und Uptime gemessen (Zero Downtime, Einhaltung von SLAs, Vermeidung von Ausfällen). Für den Betrieb bedeutet jede Änderung ein potenzielles Risiko für Störungen.

```text
       [ DEVELOPMENT ]                              [ OPERATIONS ]
  Ziel: Häufige Änderungen                    Ziel: Maximale Stabilität
  Anreiz: Neue Features                         Anreiz: Keine Ausfälle (SLA)
           |                                             |
           \------------------\       /------------------/
                              |       |
                         [ WALL OF CONFUSION ]
               - Code über die Mauer werfen ("Throw over the wall")
               - "Auf meiner Maschine hat es funktioniert!"
               - Gegenseitiges Fingerpointing bei Ausfällen
```

Die «Wall of Confusion» (Mauer der Unwissenheit)

Mit der «Wall of Confusion» bezeichnet man die organisatorische und kommunikative Trennwand zwischen Dev und Ops:

Throw it over the wall: Entwickler schreiben Code in ihrer isolierten Entwicklungsumgebung, paketieren ihn und übergeben ihn ohne tiefes Verständnis für die Produktionsinfrastruktur an den Betrieb.
Fehlender Kontext im Betrieb: Der Betrieb erhält Softwarepakete, für die es an aussagekräftiger Dokumentation, Fehlerdiagnose-Routinen oder Health Checks fehlt. Tritt ein Fehler auf, muss Ops mitten in der Nacht debuggen, ohne den Quellcode zu kennen.
Fingerpointing: Stürzt ein System ab, argumentiert Dev: «Auf meiner lokalen Maschine lief alles fehlerfrei, eure Server-Konfiguration ist fehlerhaft!». Ops entgegnet: «Euer Code leckt Speicher und wirft unbehandelte Ausnahmen, wir rollen nichts mehr aus!». DevOps reisst diese Mauer durch gemeinsame Teams, gemeinsame Ziele und geteilte Telemetrie ein.

### 3. Nennen Sie drei verbreitete Missverständnisse über DevOps und erklären Sie jeweils, warum sie falsch sind.

Missverständnis 1: «DevOps bedeutet, dass wir keine Ops-Spezialisten mehr brauchen (Entwickler machen jetzt alles selbst)»
Warum falsch? Moderner Systembetrieb, Hochverfügbarkeit, Netzwerksicherheit, Backup-Orchestrierung, Observability und Cloud-Finanzoptimierung erfordern tiefgehendes Spezialwissen. DevOps schafft Operations nicht ab, sondern wandelt dessen Rolle: Ops-Ingenieure arbeiten heute oft als Plattform-Engineers oder Site Reliability Engineers (SRE), die Werkzeuge und standardisierte Plattformen bereitstellen, damit Entwickler ihre Software sicher deployen und überwachen können.
Missverständnis 2: «DevOps ist eine Software oder eine Toolchain, die man kaufen kann»
Warum falsch? Man kann weder «ein Kilo DevOps» kaufen noch durch die blosse Einführung von GitLab CI, Terraform oder Kubernetes automatisch «DevOps machen». Wenn Teams weiterhin in getrennten Silos arbeiten, sich gegenseitig die Schuld zuschieben und keine Feedbackschleifen etablieren, beschleunigt die Toolchain lediglich die Fehlerausbreitung («Bad culture automated is just bad culture at scale»).
Missverständnis 3: «Wir richten einfach ein separates 'DevOps-Team' ein, dann ist das Problem gelöst»
Warum falsch? Wird ein dediziertes «DevOps-Team» zwischen Development und Operations geschaltet, entsteht in der Realität lediglich ein drittes Silo («Wall of Confusion 2.0»). Anstatt dass Entwickler und Betriebler direkt zusammenarbeiten, schieben nun beide Seiten Tickets an das DevOps-Team ab, wodurch Durchlaufzeiten verlängert und Verantwortlichkeiten weiter verwässert werden.

## [B1-K02] CALMS

Das CALMS-Framework wurde von Jez Humble, Damon Edwards und John Willis geprägt, um die fünf wesentlichen Säulen einer ganzheitlichen DevOps-Transformation greifbar und messbar zu machen.

### 1. Was bedeuten Culture, Automation, Lean, Measurement und Sharing im DevOps-Kontext?

C – Culture (Kultur): Kultur bildet das Fundament. Sie umfasst gemeinsame Werte, gegenseitiges Vertrauen, psychologische Sicherheit und die geteilte Verantwortung (Shared Ownership) für das gesamte System. Fehler werden als Lernchancen begriffen (No-Blame-Kultur), statt Schuldige zu suchen.
A – Automation (Automatisierung): Wiederkehrende, fehleranfällige manuelle Aufgaben werden in reproduzierbare Code-Pipelines überführt (Continuous Integration, Continuous Delivery, Infrastructure as Code, automatisierte Tests). Automatisierung schafft verlässliche Qualität, Konsistenz und befreit Fachkräfte von repetitiver Routinearbeit.
L – Lean (Schlanke Prozesse): Entlehnt aus der Lean-Manufacturing-Philosophie (Toyota Production System). Ziel ist es, Verschwendung (Waste) im Entwicklungsprozess zu eliminieren, Arbeit in kleinen Paketen (Small Batches) fliessen zu lassen, unfertige Arbeit (Work in Progress / WIP) strikt zu begrenzen und Durchlaufzeiten zu minimieren.
M – Measurement (Messung & Telemetrie): Entscheidungen werden datenbasiert und nicht nach Bauchgefühl getroffen. Erfasst werden sowohl technische Metriken (Latenz, Fehlerrate, CPU-Auslastung) als auch Prozess- und Leistungsmetriken (wie die vier DORA-Metriken: Deployment Frequency, Lead Time for Changes, Change Failure Rate, Time to Restore Service).
S – Sharing (Wissensaustausch & Transparenz): Wissen, Erfolge und Misserfolge werden über Team- und Abteilungsgrenzen hinweg geteilt. Erkenntnisse aus Störungen (Postmortems), Architekturrichtlinien und Tools stehen allen Mitarbeitenden offen zur Verfügung, um Inselwissen zu vermeiden.

### 2. Nennen Sie zu jedem Bereich ein Beispiel aus einem Softwareteam.

| CALMS-Säule | Konkretes Praxisbeispiel aus einem Softwareteam |
| --- | --- |
| Culture | Entwickler und Ops-Spezialisten nehmen gemeinsam an der Rufbereitschaft (On-Call-Rotation) teil und moderieren Störungsanalysen ohne persönliche Schuldzuweisungen (Blameless Postmortems). |
| Automation | Eine CI/CD-Pipeline führt bei jedem Git-Push automatisch Linter, Unit- und Integrationstests aus, baut ein Docker-Image und rollt dieses auf der Staging-Umgebung aus, sobald alle Tests grün sind. |
| Lean | Das Team beschränkt auf seinem Kanban-Board die maximale Anzahl gleichzeitiger Aufgaben (WIP-Limit) auf zwei Tasks pro Person und zerlegt grosse Release-Pakete in kleine Pull Requests, die in weniger als 2 Tagen gemergt werden. |
| Measurement | Das Team visualisiert die Systemgesundheit und Geschäftsdaten auf einem Grafana-Dashboard: Es überwacht kontinuierlich Fehlerraten (HTTP 5xx), Antwortzeiten (P99-Latenz) und die DORA-Kennzahl Time to Restore Service (TTRS). |
| Sharing | Nach einem grösseren Ausfall verfasst das Team ein ausführliches, öffentliches Postmortem-Dokument und stellt die Learnings in einem internen «Engineering Brown-Bag-Lunch» allen Kollegen vor. |

## [B1-K03] The Three Ways

The Three Ways (Die Drei Wege) wurden von Gene Kim, Kevin Behr und George Spafford (u.a. in The Phoenix Project und The DevOps Handbook) formuliert. Sie beschreiben die fundamentalen Prinzipien, aus denen alle DevOps-Verhaltensweisen und -Muster abgeleitet werden.

```text
+-----------------------------------------------------------------------------------+
|                           THE THREE WAYS (Gene Kim)                               |
+-----------------------------------------------------------------------------------+
 1. FIRST WAY: FLOW (Fluss von links nach rechts)
    [ Business / Dev ]  ========================>  [ Ops / Kunde ]
    - Schneller Wertstrom, kleine Batches, WIP begrenzen, Flaschenhälse abbauen.
 2. SECOND WAY: FEEDBACK (Rückkopplung von rechts nach links)
    [ Business / Dev ]  <========================  [ Ops / Kunde ]
    - Schnelle Rückmeldung, Qualität an der Quelle, Fehler sofort sichtbar machen.
 3. THIRD WAY: CONTINUAL LEARNING & EXPERIMENTATION (Lernen & Praxis)
    [ (  Wiederholung, Experimentierfreude, Game Days, Chaos Engineering  ) ]
    - Kultur des risikofreudigen Lernens, Fehlertoleranz, Meisterschaft durch Übung.
+-----------------------------------------------------------------------------------+
```

### 1. Was besagen First Way, Second Way und Third Way?

**The First Way – Principles of Flow (Fluss):**
Der Erste Weg befasst sich mit der Beschleunigung des Arbeitsflusses von links nach rechts – also von den Anforderungen (Business/Dev) über die Entwicklung, Integration und Bereitstellung bis zum Kunden und Betrieb.
Ziel ist es, die Durchlaufzeit (Lead Time) drastisch zu verkürzen.
Massnahmen: Arbeit sichtbar machen (Kanban), Losgrössen verkleinern (Small Batches), unfertige Arbeit reduzieren (WIP-Limits), Übergaben (Handoffs) minimieren und Engpässe systematisch identifizieren und auflösen.
**The Second Way – Principles of Feedback (Schnelle Rückkopplung):**
Der Zweite Weg etabliert schnelle, konstante und ununterbrochene Feedbackschleifen von rechts nach links – also vom Betrieb und den Endnutzern zurück zu den Entwicklern.
Ziel ist es, Probleme und Qualitätsmängel unmittelbar an dem Ort sichtbar zu machen, an dem sie entstehen, um sie sofort zu beheben (Prinzip der Andon-Leine aus der Lean-Produktion: Die Fertigungslinie wird gestoppt, sobald ein Fehler auftritt).
Verhindert, dass Mängel nachgelagert zu teuren Katastrophen heranwachsen.
The Third Way – Principles of Continual Learning and Experimentation (Kontinuierliches Lernen und Experimentieren):
Der Dritte Weg schafft eine Unternehmenskultur, die auf zwei Säulen beruht: kontinuierliches Experimentieren (inklusive Risikobereitschaft und Akzeptanz des Scheiterns) und das Verständnis, dass Wiederholung und Praxis die Voraussetzung für Meisterschaft sind.
Zeit wird reserviert, um technische Schulden abzubauen, Resilienz aktiv durch Stresstests zu trainieren und aus jedem Incident systematisch organisatorisches Wissen aufzubauen.

### 2. Nennen Sie zu jedem Way ein Beispiel.

**Beispiel für den First Way:**
Ein Team stellt von monatlichen manuellen Riesen-Releases auf Trunk-Based Development mit automatisiertem Continuous Deployment um. Jeder kleine Commit läuft durch eine automatisierte Pipeline und ist innerhalb von 15 Minuten produktiv, ohne dass Freigabekomitees (Change Advisory Boards) den Fluss bremsen.
**Beispiel für den Second Way:**
Nach dem Deployment eines neuen Services schlägt ein automatischer synthetischer Health Check fehl oder die Fehlerrate steigt um mehr als 1 %. Das Monitoring-System alarmiert die Entwickler per Instant Notification innerhalb von 30 Sekunden und leitet automatisch ein Canary-Rollback ein, bevor Kunden nennenswert beeinträchtigt werden.
**Beispiel für den Third Way:**
Ein Team führt einmal im Monat einen organisierten Game Day oder Chaos-Engineering-Test durch (z.B. mit Werkzeugen wie Chaos Mesh oder Simian Army): Dabei wird im Staging-Cluster absichtlich der Ausfall des primären Datenbankknotens simuliert, um zu überprüfen, ob das automatische Failover greift und das Team im Ernstfall ruhig reagieren kann.

## [B1-K04] Vergleich CALMS und Three Ways

Sowohl CALMS als auch The Three Ways sind international etablierte Referenzmodelle zur Beschreibung von DevOps. Obwohl sie aus unterschiedlichen Impulsen entstanden sind, ergänzen sie sich gegenseitig.

### 1. Wo meinen CALMS und The Three Ways dasselbe? Nennen Sie mindestens zwei Gemeinsamkeiten.

**Kultur der Fehlertoleranz und des Lernens:**
Die CALMS-Säulen Culture und Sharing decken sich inhaltlich direkt mit dem Third Way. Beide Modelle betonen ausdrücklich, dass technologische Werkzeuge wertlos sind, wenn eine Organisation von Angst, Schuldzuweisungen und Informationssilos beherrscht wird. Das Lernen aus Fehlern und das offene Teilen von Erkenntnissen (Postmortems, Retrospektiven) stehen in beiden Modellen an zentraler Stelle.
**Schnelle Feedbackschleifen durch Messung und Automatisierung:**
Die CALMS-Säulen Automation und Measurement sind die technischen Enabler für den First Way (Flow) und den Second Way (Feedback). Ohne automatisierte Tests und Deployments ist kein schneller Fluss von Code möglich; ohne kontinuierliche Messung und Monitoring existiert keine verlässliche Rückkopplungsschleife vom Betrieb zurück zur Entwicklung. Beide Modelle fordern, Probleme an der Quelle durch Daten und Automatisierung sofort aufzudecken.

### 2. Worin unterscheiden sich die beiden Modelle?

| Merkmal | CALMS-Framework | The Three Ways |
| --- | --- | --- |
| Urheber | Jez Humble, Damon Edwards, John Willis | Gene Kim, Kevin Behr, George Spafford |
| Konzeptioneller Ansatz | Taxonomisches Reifegrad- und Kriterienmodell: 5 gleichwertige, thematische Säulen | Dynamisches Prozess- und Prinzipienmodell: Richtungs- und wertstromorientiert |
| Struktur | Kategorisierend (C, A, L, M, S stehen nebeneinander als Analysebereiche) | Vektoriell / Flussorientiert (1. Links->Rechts, 2. Rechts->Links, 3. Schleife/Kreis) |
| Typischer Einsatzzweck | Standortbestimmung & Auditierung: Wo stehen wir als Organisation in Bezug auf Kultur, Werkzeuge und Metriken? | Handlungsanleitung für Engineering & Flow: Wie gestalten wir unsere täglichen Arbeitsabläufe, Pipelines und Übungen? |
| Fokus | Hebt Sharing und Kultur als explizite Prüfkriterien hervor | Hebt den Wertstrom (Flow) und die Übungspraxis (Mastery through Practice) hervor |

## [B1-K05] Monitoring und Alarme

Monitoring und Observability sind das sensorische Nervensystem moderner Softwareplattformen. Ohne kontinuierliche Beobachtung agieren Entwicklung und Betrieb im Blindflug.

### 1. Wozu dienen Health Check, Logs, Metriken und Alarme?

```text
+-----------------------------------------------------------------------------------+
|                        BAUSTEINE DES MONITORINGS                                  |
+-----------------------------------------------------------------------------------+
| 1. HEALTH CHECK   -> "Lebt der Service gerade?"        (Schnell, binär/Status)     |
| 2. METRIKEN       -> "Wie verhält sich das System?"    (Numerisch, Trends, DORA)   |
| 3. LOGS           -> "Was ist exakt passiert?"         (Detailliert, Kontext, RCA) |
| 4. ALARME         -> "Achtung, Mensch muss handeln!"   (Actionable Benachrichtigung)
+-----------------------------------------------------------------------------------+
```

**Health Check:**
Ein automatisierter, meist leichtgewichtiger Endpunkt (z.B. GET /health oder /healthz), der Auskunft über den Betriebszustand einer Instanz gibt (z.B. Liveness: Läuft der Prozess noch? Readiness: Sind Datenbankverbindungen aufgebaut und Anfragen annehmbar?).
Zweck: Dient Load Balancern und Container-Orchestratoren (z.B. Kubernetes), um ungesunde Instanzen automatisch aus dem Datenverkehr zu entfernen oder neu zu starten (Self-Healing).
**Logs (Protokolldaten):**
Diskrete, zeitstempelbehaftete Textnachrichten über einzelne aufgetretene Ereignisse (Events), oft angereichert mit Kontext (Trace-ID, User-ID, Error Stack Traces).
Zweck: Dienen der detaillierten Ursachenforschung (Root Cause Analysis) im Fehlerfall, um den genauen Ablauf eines Scheiterns nachvollziehen zu können.
**Metriken (Metrics):**
Numerische, über feste Zeitintervalle aggregierbare Messwerte (z.B. CPU-Auslastung in %, Anzahl HTTP-Requests pro Sekunde, Fehlerquote in %, Speicherverbrauch, P95/P99-Latenz).
Zweck: Ermöglichen die Visualisierung von Trends auf Dashboards, Kapazitätsplanung und das mathematische Auslösen von Schwellenwert-Alarmen, da sie extrem effizient gespeichert und abgefragt werden können.
**Alarme (Alerts):**
Automatisierte Mitteilungen, die an Personen oder Rufbereitschaften gesendet werden (z.B. per PagerDuty, SMS oder Slack), wenn Metriken oder Health Checks definierte Schwellenwerte verletzen.
Zweck: Unterbrechen Menschen gezielt, wenn das System ein Problem nicht selbstständig lösen kann und unmittelbare menschliche Intervention erforderlich ist, um einen Ausfall abzuwenden.

### 2. Was macht einen Alarm nützlich, und wie hängt Monitoring mit dem Second Way zusammen?

Was macht einen Alarm nützlich? (Qualitätskriterien)

Ein schlechter Alarm führt zu Alert Fatigue (Alarmmüdigkeit), bei der Warnungen ignoriert werden. Ein nützlicher Alarm erfüllt folgende Kriterien:

Actionable (Handlungsorientiert): Jeder ausgelöste Alarm erfordert eine zwingende, zeitnahe Handlung durch einen Menschen. Gibt es nichts zu tun (reine Information), gehört die Nachricht in ein Logbuch oder Dashboard, nicht in einen Alarm.
Symptom- bzw. benutzerorientiert statt Ursachen-Spam: Alarmiert wird primär auf die Auswirkungen für den Anwender (z.B. «Kunden erhalten bei 8% der Checkouts HTTP-500-Fehler») statt auf abstrakte Teilursachen (z.B. «Server 4 hat 85% CPU»).
Verknüpft mit Dokumentation (Runbook): Der Alarm enthält einen direkten Link zu einem aktuellen Runbook / Playbook, das konkrete Schritte zur Diagnose und Entstörung beschreibt.
Geringe Fehlalarmrate (Low False-Positive Rate): Schwellenwerte und Hysteresen sind so kalibriert, dass vorübergehende, unkritische Spikes keine Nachtruhe stören.

Zusammenhang zwischen Monitoring und dem Second Way

Der Second Way (Rückkopplung) verlangt schnelle Feedbackschleifen von der Produktion zurück zur Entwicklung:

Monitoring ist die technische Realisierung dieser Rückkopplung: Es schliesst den Kreis, indem es in Echtzeit misst, wie sich neuer Code unter Produktionslast verhält.
Treten nach einem Release Fehler, Performanceeinbrüche oder Abstürze auf, meldet das Monitoring dies sofort an das verantwortliche Team.
Dadurch wird das Prinzip «Qualität an der Quelle sichern» verwirklicht: Probleme werden abgefangen, bevor sie sich über Stunden oder Tage unbemerkt im Gesamtsystem festsetzen.

## [B1-K06] Incidents, Blameless-Kultur und Postmortem

Störungen in komplexen IT-Systemen sind unvermeidbar. Der entscheidende Unterschied liegt darin, wie eine Organisation mit Störungen umgeht.

### 1. Wie läuft die Bearbeitung eines Incidents ab, und was hat dabei Vorrang?

Phasen der Incident-Bearbeitung

```text
[ 1. Erkennung ]       -> Alarm schlägt an / Monitoring schlägt aus
      |
[ 2. Triage & Rolle ]  -> Schweregrad festlegen, Incident Commander bestimmen
      |
[ 3. EINDÄMMUNG ]      -> VORRANG: Dienst schnellstmöglich wiederherstellen!
      |                   (Rollback, Restart, Failover, Traffic-Drosselung)
[ 4. Lösung & Entwarnung]-> System läuft stabil, Stakeholder informiert
      |
[ 5. Nachbereitung ]   -> Blameless Postmortem, Ursachenanalyse, Action Items
```

Erkennung (Detection): Ein Incident wird über automatisierte Alarme oder Kundenmeldungen registriert.
Triage & Rollenverteilung: Festlegen des Schweregrads (z.B. Severity 1 = kritischer Totalausfall). Ernennung eines Incident Commanders, der die Koordination leitet, Kommunikation bündelt und das Team vor externen Störungen abschirmt.
Eindämmung & Wiederherstellung (Mitigation): Einleiten schneller Gegenmassnahmen zur Schadensbegrenzung.
Lösung & Entwarnung (Resolution): Verifikation, dass der Dienst wieder regulär antwortet; Benachrichtigung der Betroffenen.
Nachbereitung (Postmortem): Systematische Aufarbeitung nach Beruhigung der Lage.

Was hat absolute Priorität?

DIE SCHNELLE WIEDERHERSTELLUNG DES DIENSTES FÜR DIE KUNDEN (Mitigation / Minimierung der MTTR - Mean Time to Recovery) HAT ABSOLUTEN VORRANG VOR DER DETAIL-URSACHENFORSCHUNG.

Analogie zur Feuerwehr: Wenn ein Haus brennt, sucht die Feuerwehr nicht mitten im Brand nach der defekten Steckdose, sondern löscht zuerst das Feuer und rettet Menschenleben.
Für Softwareteams heisst das: Im Zweifelsfall wird sofort ein Rollback auf die letzte funktionierende Version durchgeführt, Server werden neu gestartet oder Traffic wird umgeleitet. Das genaue Studium von Speicherdumps und fehlerhaften Codezeilen geschieht erst, wenn der Kunde wieder ungestört arbeiten kann.

### 2. Was bedeutet «blameless», und was passiert in einem Team, in dem nach Schuldigen gesucht wird?

Bedeutung von «blameless» (Kultur ohne Schuldzuweisung)

**Eine Blameless-Kultur (geprägt u.a. von John Allspaw und Google SRE) beruht auf der Prämisse:**

«Wir gehen davon aus, dass jede Person zu jedem Zeitpunkt mit den ihr zur Verfügung stehenden Informationen, Werkzeugen und unter den gegebenen Umständen die bestmögliche Entscheidung treffen wollte.»

Wenn ein Mensch einen falschen Befehl eintippt oder eine fatale Fehlkonfiguration einspielt, ist dies nicht die Wurzelursache, sondern ein Symptom für systemische Schwachstellen. Die Fragen lauten daher nicht «Wer war schuld?», sondern:

Warum hat das System dem Benutzer erlaubt, diese fatale Aktion ohne Warnung durchzuführen?
Warum gab es keine Guardrails, kein Vier-Augen-Prinzip oder keine farbliche Kennzeichnung der Produktionsumgebung?
Welche Faktoren (Schlafmangel, unklare Dokumentation, Zeitdruck) haben die Fehlhandlung begünstigt?

Was passiert in einem Team, das nach Schuldigen sucht (Blame Culture)?

**Wird in einem Unternehmen nach einem Sündenbock gesucht, treten verheerende Effekte ein:**

Vertuschung und Verheimlichung: Mitarbeitende verstecken Fehler, manipulieren Logdateien oder warten ab, in der Hoffnung, dass niemand merkt, wer den Fehler verursacht hat. Dadurch werden Incidents viel zu spät gemeldet und behoben.
Innovationsstopp & Risikoaversion: Niemand wagt es mehr, Neuerungen auszuprobieren, Code zu refaktorisieren oder Produktionsänderungen vorzunehmen («Wer nichts tut, macht keine Fehler und wird nicht bestraft»).
Zementierung der Systemfehler: Da das Problem auf «menschliches Versagen» geschoben und die Person womöglich abgemahnt oder gefeuert wird, bleibt die eigentliche Falle im System bestehen. Die nächste Person wird früher oder später in denselben Fehler laufen.

### 3. Was ist ein Postmortem, und was steht darin?

Ein Postmortem (auch Post-Incident Review / PIR oder Learning Review) ist ein schriftliches Dokument, das nach der Behebung eines gravierenden Incidents im Team erstellt wird, um gemeinsam aus dem Vorfall zu lernen und das Gesamtsystem resilienter zu machen.

**Typische Bestandteile eines professionellen Postmortems:**

**Executive Summary & Metadaten:**
Kurzzusammenfassung: Was ist passiert, welche Dienste waren betroffen?
Datum, Gesamtdauer des Ausfalls, Auswirkung auf Kunden (Impact, z.B. Anzahl betroffener Anfragen, SLA-Verletzung).
**Chronologische Zeitleiste (Timeline):**
Detaillierter zeitlicher Ablauf mit präzisen Zeitstempeln: Wann trat der Fehler ein? Wann schlug der Alarm an? Wann begann die Triage? Wann wurde welche Massnahme ergriffen? Wann war der Dienst wiederhergestellt?
**Ursachenanalyse (Root Cause Analysis):**
Untersuchung der beitragenden Faktoren, häufig unter Einsatz der 5-Why-Methode (Fünfmal «Warum?» fragen, um von der vordergründigen Handlung zu den systemischen Schwachstellen vorzudringen).
**Bewertung (What went well / What went poorly):**
Was hat hervorragend funktioniert? (z.B. schnelle Alarmierung, guter Zusammenhalt im Incident-Kanal).
Wo hatten wir Glück oder Pech? Wo versagten Werkzeuge oder Dokumentation?
**Korrekturmassnahmen (Action Items):**
Konkrete, nachverfolgbare Aufgaben zur dauerhaften Fehlervermeidung. Jeder Eintrag besitzt eine eindeutige Ticket-Nummer, eine verantwortliche Person (Owner) und eine Fälligkeit (Due Date), z.B. «Automatischen Restore-Test für DB-Backups bis KW 42 implementieren (Ticket OPS-1234, Owner: Sarah)».

## [B1-K07] Fall GitLab 2017

Der Datenbankvorfall von GitLab.com am 31. Januar 2017 gilt in der weltweiten DevOps-Gemeinschaft als Paradebeispiel für moderne Fehlerkultur, das Zusammenspiel der Three Ways und die Tücken ungetesteter Sicherheitsnetze.

```text
+-----------------------------------------------------------------------------------+
|               GITLAB.COM VORFALL 2017 - SYSTEMANALYSE                             |
+-----------------------------------------------------------------------------------+
| Ausgangslage: Hohe Spam-Last -> Replikation hinkt hinterher                       |
| Aktion:       Ops-Ingenieur will Replikations-Verzeichnis auf DB2 leeren          |
| FEHLHANDLUNG: Eingeloggt auf DB1 (Primär) -> 300 GB Daten gelöscht!               |
+-----------------------------------------------------------------------------------+
| Backup-Kaskade versagt:                                                           |
| 1. DB-Dumps:      LEER! (pg_dump Version mismatch, E-Mails unbemerkt abgewiesen)  |
| 2. Snapshots:     Für DB-Server nicht aktiviert                                   |
| 3. Azure Replica: Wegen Last nicht synchron                                       |
| RETTUNG:          Manueller Snapshot eines Kollegen von vor 6 Stunden             |
+-----------------------------------------------------------------------------------+
| Reaktion:     - 100% Blameless (Ingenieur wurde ausdrücklich nicht beschuldigt)   |
|               - Volle Transparenz (Live auf YouTube, Google Doc öffentlich)       |
| Massnahmen:   - Schrödinger's Backup: Regelmässiges automatisiertes Restore       |
|               - Terminal-Prompt rot für Prod, Guardrails gegen rm -rf             |
+-----------------------------------------------------------------------------------+
```

### 1. Ordnen Sie drei Stellen des Falls je einem CALMS-Bereich zu und begründen Sie.

Stelle 1: Öffentliche Kommunikation und YouTube-Livestream
Textstelle: «GitLab entschied sich, vollständig offen zu kommunizieren: Die Wiederherstellung wurde in einem öffentlichen Dokument protokolliert und live auf YouTube übertragen. Danach veröffentlichte GitLab einen ausführlichen Postmortem-Bericht ohne Beschuldigungen.»
CALMS-Bereich: CULTURE und SHARING
Begründung: GitLab demonstrierte höchste Transparenz und gelebte Blameless-Kultur (Culture). Indem der gesamte Incident, alle Fehler und die Wiederherstellungsschritte live und unzensiert mit der Welt geteilt wurden (Sharing), verwandelte GitLab einen potenziellen PR-Super-GAU in einen unschätzbaren Vertrauens- und Wissensgewinn für die gesamte Tech-Community.
Stelle 2: Leere Datenbank-Dumps und abgewiesene E-Mail-Fehlermeldungen
Textstelle: «Die regelmässigen Datenbank-Dumps waren seit längerer Zeit leer, weil das Backup-Werkzeug eine andere Datenbankversion erwartete. Die Fehlermeldungen dazu wurden per E-Mail verschickt, aber vom Mailsystem unbemerkt abgewiesen.»
CALMS-Bereich: Versagen von AUTOMATION und MEASUREMENT
Begründung: Die Automatisierung (Automation) war fehlerhaft implementiert und besass keine Verifikationsstufe. Gleichzeitig versagte die Messung und Überwachung (Measurement): Das Mailsystem wies die Fehler unbemerkt ab, es gab keine Metriken über Dump-Dateigrössen und keinen Alarm, der anschlug, wenn ein Backup 0 Bytes gross war.
Stelle 3: Manuelle Sicherung rettet die Datenbasis
Textstelle: «Gerettet wurde die Situation durch einen Snapshot, den ein Mitarbeiter rund sechs Stunden zuvor manuell für einen anderen Zweck erstellt hatte. Automatische Disk-Snapshots waren nicht aktiviert.»
CALMS-Bereich: Mangel an LEAN und AUTOMATION
Begründung: Das Fehlen automatisierter LVM/Disk-Snapshots (Automation) führte dazu, dass sechs Stunden Arbeitsergebnisse der weltweiten Nutzer unwiderruflich vernichtet wurden. Nach Lean-Prinzipien (Lean) stellt dies gravierende Verschwendung (Waste) und einen unzuverlässigen Prozess dar, da die Datenrettung von einem reinen Glücksfall (einem manuellen Zufalls-Snapshot) abhing.

### 2. Welcher Way fehlte vor dem Vorfall, welcher wirkte danach?

Vor dem Vorfall fehlte: Der SECOND WAY (Feedback)
Es existierten keinerlei funktionierende Feedbackschleifen bezüglich der Datenintegrität und der Backups. Die Skripte liefen ins Leere, die Benachrichtigungen versandeten im Mailsystem, und niemand im Team erhielt eine Rückmeldung über den fatalen Zustand der Datensicherungen. Ebenso fehlte Feedback an der Konsole: Das Terminal gab dem Ingenieur kein visuelles Feedback darüber, dass er auf dem produktiven Primärknoten agierte.
Nach dem Vorfall wirkte: Der THIRD WAY (Kontinuierliches Lernen & Experimentieren)
GitLab nutzte das Desaster als Katalysator für organisationales Lernen. Es wurden sofort Game Days und Desaster-Recovery-Tests etabliert. Technische Barrieren wurden errichtet (u.a. farbige Bash-Prompts für Produktionsumgebungen, restriktive Rechte, Bereitstellung von Staging-Clustern mit realitätsnahen Daten), und der Vorfall wurde im Team systematisch durchgearbeitet, um langfristige Meisterschaft im Systembetrieb zu erreichen.

### 3. Ist der Ingenieur schuld? Nehmen Sie aus DevOps-Sicht Stellung.

Nein, aus DevOps- und SRE-Sicht trifft den Ingenieur absolut keine Schuld.

**Begründung aus systemischer Perspektive:**

Das System erlaubte die Katastrophe: Wenn ein einzelner, müde getippter Befehl (rm -rf) in einer SSH-Session ausreicht, um 300 GB Produktionsdaten einer globalen Plattform unwiederbringlich zu vernichten, liegt ein massives System- und Architekturversagen vor, kein persönliches Versagen. Es gab keine Sicherheitsabfragen, keine automatischen Dateisperren und kein technisches Vier-Augen-Prinzip für destruktive Operationen.
Mangelnde visuelle Ergonomie: Der Primärserver und der Sekundärserver sahen in der Konsole identisch aus. Es gab keine Signalfarbe (z.B. roter Terminalhintergrund für Produktion) und keinen Hostnamen-Schutz.
Erhöhte Belastung und Erschöpfung: Der Ingenieur arbeitete spät in der Nacht unter enormem Stress, hervorgerufen durch einen akuten Spam-Angriff und Replikationsprobleme. Dass Menschen unter Schlafmangel und Stress Fehler machen, ist biologische Realität; robuste Systeme müssen dafür Schutzplanken (Guardrails) bereitstellen.
Falsches Sicherheitsgefühl durch Scheinsicherheiten: Die eigentliche Ursache für den 18-stündigen Ausfall und Datenverlust war nicht das Löschen der Daten, sondern dass die fünf vorhandenen Backup-Mechanismen nicht funktionierten. Daran trug der einzelne Operator keine Schuld – dies war ein kollektives Versäumnis der organisatorischen Verifikationsprozesse.

### 4. Welche Massnahme verhindert dauerhaft, dass Backups unbemerkt wertlos sind?

Das Prinzip von «Schrödinger's Backup»

«Der Zustand jedes Backups ist unbekannt und unbestimmt (sowohl lebendig als auch tot), bis der Versuch unternommen wird, es erfolgreich wiederherzustellen.»

Die einzig wirksame Massnahme: Vollautomatisierte Wiederherstellungstests (Continuous Automated Restore Testing)

Ein Backup existiert nicht, wenn man lediglich Dateien auf ein Speichermedium kopiert. Ein Backup existiert erst dann, wenn seine erfolgreiche Wiederherstellung automatisiert und regelmässig bewiesen wurde.

**Die dauerhafte Lösung besteht aus folgenden automatisierten Pipeline-Schritten:**

Automatisierter Restore-Job (z.B. täglich oder wöchentlich): Ein automatisierter Cron-Job nimmt das jüngste Backup-Image.
Isolierte Testumgebung hochfahren: In einer temporären Sandbox (z.B. Ephemeral Container oder isolierte VM) wird ein neuer Datenbankserver auf Basis des Backups gestartet.
**Automatisierte Smoke- & Integritätstests:**
Prüfung von Tabellenstrukturen und Konsistenzchecks (pg_isready, Checksums).
Ausführen von SQL-Abfragen: Zählen von Datensätzen in Schlüssel-Tabellen (z.B. SELECT COUNT(*) FROM users;). Ist die Anzahl 0 oder weicht sie stark vom Vortag ab, bricht der Test ab.
**Metrik & Alarmierung:**
Bei erfolgreichem Durchlauf wird ein Zeitstempel an das Monitoring gesendet (Heartbeat / Dead Man's Snitch).
Bleibt das Signal aus oder schlägt die Wiederherstellung fehl, wird sofort ein PagerDuty-Alarm mit höchster Priorität ausgelöst.
Ressourcen aufräumen: Nach erfolgreicher Verifikation wird die temporäre Instanz wieder gelöscht.

## Quellen

Die in diesem Dokument erarbeiteten Inhalte basieren auf folgenden fachlichen Quellen und Werkzeugen:

GitLab Inc.: Postmortem of database outage of January 31, 2017. Offizieller technischer Bericht (Postmortem of database outage of January 31).
Gene Kim, Jez Humble, Patrick Debois, John Willis: The DevOps Handbook: How to Create World-Class Agility, Reliability, and Security in Technology Organizations. IT Revolution Press.
Gene Kim, Kevin Behr, George Spafford: The Phoenix Project: A Novel About IT, DevOps, and Helping Your Business Win. IT Revolution Press.
Betsy Beyer, Chris Jones, Jennifer Petoff, Niall Richard Murphy (Google SRE Team): Site Reliability Engineering: How Google Runs Production Systems. O'Reilly Media (Kapitel: Monitoring Distributed Systems und Postmortem Culture: Learning from Failure).
Damon Edwards, John Willis: CALMS Framework for DevOps Assessment (Atlassian DevOps Resources, CALMS Framework | Atlassian).
TBZ Technische Berufsschule Zürich: Modul 324 DevOps – Handout «Der Weg einer Änderung» und Kriterienkatalog B1-K01 bis B1-K07.
KI-Werkzeug: Google AI Studio (Gemini 2.5 / 3.8) – Unterstützend eingesetzt zur strukturierten Synthese der Leitfragen und Erstellung vergleichender Tabellen, fachlich gegengelesen und geprüft gemäss Kriterienkatalog.
