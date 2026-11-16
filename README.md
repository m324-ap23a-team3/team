# Team 3: Ticketsystem (Projekt 4)

M324, Klasse ap23a. Team-Repository: Theorie, Dokumentation, Entscheidungen und Übersicht.

## Mitglieder

| Name | Account |
|---|---|
| Nico | [@NicoM77](https://github.com/NicoM77) |
| Ardit | [@ardit-kashtanjeva](https://github.com/ardit-kashtanjeva) |
| Gabriel | [@gabrielgreco11](https://github.com/gabrielgreco11) |
| Eduard | [@Eduard452](https://github.com/Eduard452) |

Alle sind im GitHub-Team [`devops`](https://github.com/orgs/m324-ap23a-team3/teams/devops) und haben in allen Repos dieselben Rechte (Maintain). Es gibt keine getrennten Rollen.
Lehrperson: [@yvesnussletbz](https://github.com/yvesnussletbz) (Owner)

## Entscheidungen

- [Kickoff-Entscheidungen (Ticket #1)](https://github.com/m324-ap23a-team3/team/issues/1)

## Theorie

- [Theorie B1–B4](./theorie/B-Theorie-Aufträge/)

## System

| | Link |
|---|---|
| Zeitlogging | _folgt_ |
| Board | _folgt_ |
| Repositories | [ms1](https://github.com/m324-ap23a-team3/ms1) · [ms2](https://github.com/m324-ap23a-team3/ms2) |
| Dokumentation | [docs/](./docs/) · [Meetings und Arbeitsprozesse](./docs/Meetings_Arbeitsprozesse.md) |
| Produktion | (ab P4) |

`main` ist in `ms1` und `ms2` geschützt: Änderungen nur über Pull Request mit 1 Approval und grünem Check `test`.
Das Repo [infra](https://github.com/m324-ap23a-team3/infra) ist ein Platzhalter. Wo die Deployment-Konfiguration liegt, entscheiden wir in P4.

## Applikation (Kurzfassung)

- **MS1 – Mitarbeiter:** Vorname, Nachname, Beitrittsdatum, Skilllevel (1–5), ID automatisch. US1 erfassen, US2 alle auslesen.
- **MS2 – Tickets:** Status (Open, In Progress, Review, Done), Titel, Beschreibung, Review-Datum und Done-Datum (UTC, nur bei passendem Statuswechsel), Mitarbeiter (Pflicht). US3 erfassen; der Mitarbeiter wird **über den Endpunkt von MS1** geprüft, nicht über dessen Datenbank.
- Spezifikation: [Projekt4_Ticketsystem.md](https://gitlab.com/ch-tbz-it/Stud/m324/-/blob/nuy/Projekt/Applikationen/Projekt4_Ticketsystem.md)

## Links

- Auftrag Projekt: [Projekt/Readme.md](https://gitlab.com/ch-tbz-it/Stud/m324/-/blob/nuy/Projekt/Readme.md)
- Miro: <https://miro.com/app/board/uXjVEQDH4SM=/>
