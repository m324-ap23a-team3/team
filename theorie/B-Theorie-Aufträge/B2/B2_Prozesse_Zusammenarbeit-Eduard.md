# B2 – Prozesse und Zusammenarbeit

## [B2-K01] Vom SDLC zum DevOps Lifecycle

### 1. Welche Phasen hat der DevOps Lifecycle und warum ist er ein Kreislauf?

1. Plan – Anforderungen festlegen.
2. Code – Software programmieren.
3. Build – Programm erstellen.
4. Test – Software testen.
5. Release – Version freigeben.
6. Deploy – Software veröffentlichen.
7. Operate – Software betreiben.
8. Monitor – Leistung überwachen.

Es ist ein Kreislauf, weil Feedback und Fehler wieder in die Planung einfliessen.

### 2. Drei Unterschiede zu Wasserfall

- Wasserfall: Entwicklung und Betrieb getrennt. DevOps: Zusammenarbeit.
- Wasserfall: Grosse, seltene Releases. DevOps: Kleine, häufige Releases.
- Wasserfall: Tests oft spät. DevOps: Kontinuierliche Tests.

### 3. Warum sind kleine Änderungen weniger riskant?

- Fehler sind einfacher zu finden.
- Probleme können schneller behoben werden.
- Änderungen lassen sich leichter rückgängig machen.
- Das Ausfallrisiko sinkt.

---

## [B2-K02] MVP und Walking Skeleton

### 1. Was ist ein MVP?

**MVP (Minimum Viable Product):** Erste nutzbare Version mit den wichtigsten Funktionen.

Zweck: Früh Feedback erhalten, Kosten sparen und die Idee testen.

### 2. Was ist ein Walking Skeleton?

Eine einfache technische Grundstruktur, bei der alle Komponenten verbunden sind.

Beispiel: Benutzeroberfläche, Backend und Datenbank.

**Warum zuerst?**
- Technische Probleme früh erkennen.
- Zusammenspiel der Komponenten testen.
- Grundlage für weitere Funktionen schaffen.

**Unterschied:**
- MVP = Nutzen für Benutzer.
- Walking Skeleton = Technische Grundlage.

---

## [B2-K03] Anforderungen und Tickets

### 1. Wie ist eine User Story aufgebaut?

Aufbau:

> Als [Benutzer] möchte ich [Funktion], damit [Nutzen].

Beispiel:

> Als Kunde möchte ich meine Buchung stornieren können, damit ich Termine absagen kann.

Akzeptanzkriterien:
1. Der Buchungsstatus wird auf «Storniert» gesetzt.
2. Der Kunde erhält eine Bestätigungsmail.

### 2. Vertikaler und horizontaler Schnitt

- **Horizontal:** Aufteilung nach Technik, z. B. Datenbank, Backend und E-Mail.
- **Vertikal:** Aufteilung nach vollständigen Benutzerfunktionen.

Vertikal ist besser, weil Funktionen früher nutzbar und testbar sind und schneller Feedback entsteht.

### 3. Welche Ticketarten gibt es?

- **Feature:** Neue Funktion.
- **Bug:** Fehler beheben.
- **Incident:** Akute Störung behandeln.
- **Change:** Bestehende Funktion ändern.

**Ticketvorlagen** sorgen für einheitliche und vollständige Informationen, z. B. Beschreibung, Priorität und Akzeptanzkriterien.

---

## [B2-K04] Kanban und Definition of Done

### 1. Was sind die Grundprinzipien von Kanban?

- Arbeit auf einem Board visualisieren.
- WIP-Limits setzen.
- Engpässe erkennen.
- Arbeitsfluss verbessern.
- Prozesse kontinuierlich optimieren.

### 2. Wozu dienen WIP-Limits?

WIP = Work in Progress.

WIP-Limits begrenzen gleichzeitig bearbeitete Aufgaben. Dadurch wird Überlastung verhindert.

Ein Engpass ist erkennbar, wenn sich viele Aufgaben in einer Spalte sammeln.

### 3. Was gehört in eine DevOps-Definition-of-Done?

Zusätzlich zu fertigem Code und erfolgreichen Tests:

- Peer Review durchgeführt.
- Automatisierte Tests erfolgreich.
- Sicherheitsprüfung durchgeführt.
- Monitoring vorhanden.
- Dokumentation aktualisiert.
- Deployment und Rollback möglich.

Ziel: Die Software soll nicht nur funktionieren, sondern auch zuverlässig betrieben werden können.

---

## [B2-K05] Nachvollziehbare Entwicklung im Team

### 1. Wie hängen die Entwicklungsschritte zusammen?

1. **Ticket:** Aufgabe beschreiben.
2. **Branch:** Eigenen Entwicklungszweig erstellen.
3. **Commit:** Codeänderung speichern.
4. **Merge Request:** Änderung zur Übernahme vorschlagen.
5. **Review:** Code überprüfen.
6. **Pipeline:** Build und Tests automatisieren.
7. **Deployment:** Software bereitstellen.

**Ablauf:**

Ticket → Branch → Commit → Merge Request → Review → Pipeline → Deployment

Zweck: Änderungen bleiben nachvollziehbar und kontrollierbar.

### 2. Wozu dient ein Peer Review?

Eine andere Person überprüft den Code.

Vorteile:
- Fehler finden.
- Codequalität verbessern.
- Wissen teilen.
- Zusammenarbeit fördern.
- Einheitliche Standards sichern.

---

## [B2-K06] DORA-Kennzahlen

### 1. Was messen die vier DORA-Kennzahlen?

- **Deployment Frequency:** Wie oft wird veröffentlicht?
- **Lead Time for Changes:** Zeit vom Commit bis zum Release.
- **Change Failure Rate:** Anteil der Releases mit Fehlern.
- **Time to Restore:** Zeit zur Behebung einer Störung.

Formel:

`Change Failure Rate = (Fehlerhafte Deployments / Alle Deployments) × 100`

### 2. Warum gehören Tempo und Stabilität zusammen?

Häufige Releases bringen Funktionen schneller zu Nutzern. Gute Tests und Monitoring verhindern Fehler.

Ziel: Schnell und zuverlässig veröffentlichen.

### 3. Warum nicht einzelne Personen beurteilen?

- Softwareentwicklung ist Teamarbeit.
- Engpässe können organisatorische Ursachen haben.
- Kennzahlen sollen Prozesse verbessern, nicht Mitarbeitende unter Druck setzen.

---

## [B2-K07] Fall Pixelwerk

### 1. Berechnen Sie die vier DORA-Kennzahlen.

**Deployment Frequency:**

2 Releases in 2 Monaten = **1 Release pro Monat**.

Bewertung: Zu selten.

**Lead Time for Changes:**

3. März bis 5. April = **33 Tage**.

Bewertung: Zu lange.

**Change Failure Rate:**

2 fehlerhafte Releases / 2 Releases × 100 = **100 %**.

Bewertung: Beide Releases benötigten einen Hotfix.

**Time to Restore:**

(6 + 14) / 2 = **10 Stunden**.

Bewertung: Störungen werden relativ langsam behoben.

**Zusammenfassung:**

| Kennzahl | Ergebnis |
|---|---:|
| Deployment Frequency | 1 pro Monat |
| Lead Time | 33 Tage |
| Change Failure Rate | 100 % |
| Time to Restore | 10 Stunden |

### 2. Wo ist der Engpass?

**Bereit für Deploy:** 11 Aufgaben warten auf die Bereitstellung.

Grund: Nur ein Release pro Monat und nur eine Person im Betrieb.

### 3. Welches WIP-Limit schlagen Sie vor?

**Vorschlag: 4 Aufgaben.**

Begründung:
- 4 Entwickler = 4 Aufgaben.
- Weniger parallele Arbeit.
- Mehr Fokus.
- Aufgaben werden schneller abgeschlossen.

### 4. Kritik am Schnitt und drei vertikale Stories

Die bisherigen Tickets (DB-Tabellen, Backend-Logik, Mail-Versand) sind horizontal aufgeteilt.

Problem: Keine Aufgabe liefert allein einen vollständigen Kundennutzen.

**Bessere Lösung: Vertikale User Stories.**

**Story 1: Buchung stornieren**

> Als Kundin möchte ich meine Buchung stornieren, damit ich einen Termin absagen kann.

- Buchung wird storniert.
- Status wird aktualisiert.

**Story 2: Rückerstattung erhalten**

> Als Kundin möchte ich mein Geld zurückerhalten, damit ich bei einer berechtigten Stornierung nicht bezahlen muss.

- Rückerstattung wird ausgelöst.
- Status wird gespeichert.

**Story 3: Bestätigungsmail erhalten**

> Als Kundin möchte ich eine Bestätigungsmail erhalten, damit ich einen Nachweis habe.

- E-Mail wird versendet.
- Buchungsnummer und Stornierung stehen in der Mail.

**Vorteile:** Früher testen, schneller Feedback erhalten und Fehler früher erkennen.

---

## Quellen

- [DORA – DORA-Metriken](https://dora.dev/guides/dora-metrics-four-keys/)
- [Atlassian – Kanban](https://www.atlassian.com/de/agile/kanban)
- [Atlassian – User Stories](https://www.atlassian.com/de/agile/project-management/user-stories)
- [Google Cloud – DevOps](https://cloud.google.com/devops)
- [Martin Fowler – Continuous Integration](https://martinfowler.com/articles/continuousIntegration.html)