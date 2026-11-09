# M324 – Team ap23a-3 · Projekt 4: Ticketsystem

Team-Repository: Theorie-Abgaben, Kickoff-Entscheidungen und Übersicht.

## Mitglieder und Teilgruppen

| Name | GitHub | Teilgruppe | Phase A | Phase B |
|---|---|---|---|---|
| Nico | [@NicoM77](https://github.com/NicoM77) | A | Dev | Ops |
| Ardit | [@ardit-kashtanjeva](https://github.com/ardit-kashtanjeva) | A | Dev | Ops |
| Gabriel | [@gabrielgreco11](https://github.com/gabrielgreco11) | B | Ops | Dev |
| Eduard | [@Eduard452](https://github.com/Eduard452) | B | Ops | Dev |

GitHub-Teams: [`dev`](https://github.com/orgs/m324-ap23a-team3/teams/dev) (Teilgruppe A) und [`ops`](https://github.com/orgs/m324-ap23a-team3/teams/ops) (Teilgruppe B)
Lehrperson: [@yvesnussletbz](https://github.com/yvesnussletbz) (Owner)

## Repositories

| Repo | Inhalt | `dev` | `ops` |
|---|---|---|---|
| [team](https://github.com/m324-ap23a-team3/team) | Theorie (`theorie/`), Entscheidungen, Übersicht | Write | Write |
| [ms1](https://github.com/m324-ap23a-team3/ms1) | MS1 Mitarbeiter: .NET 10 Web API, Tests, Dockerfile, Build-Pipeline | Maintain | Write |
| [ms2](https://github.com/m324-ap23a-team3/ms2) | MS2 Tickets: .NET 10 Web API, Tests, Dockerfile, Build-Pipeline | Maintain | Write |
| [infra](https://github.com/m324-ap23a-team3/infra) | Deployment, Deploy-Pipeline, Secrets, Monitoring, Runbook | Write | Maintain |

`main` ist in allen Repos geschützt: Änderungen nur über Pull Request. In `ms1`/`ms2` darf nur `dev` mergen, in `infra` nur `ops`.

## Projekt 4: Ticketsystem (Kurzfassung)

- **MS1 – Mitarbeiter:** Vorname, Nachname, Beitrittsdatum, Skilllevel (1–5), ID automatisch. US1 erfassen, US2 alle auslesen.
- **MS2 – Tickets:** Status (Open, In Progress, Review, Done), Titel, Beschreibung, Review-Datum und Done-Datum (UTC, nur bei passendem Statuswechsel), Mitarbeiter (Pflicht). US3 erfassen; der Mitarbeiter wird **über den Endpunkt von MS1** geprüft, nicht über dessen Datenbank.
- Auftrag: [Projekt4_Ticketsystem.md](https://gitlab.com/ch-tbz-it/Stud/m324/-/blob/nuy/Praxis/Applikationen/Projekt4_Ticketsystem.md)

## Links

- Kickoff-Entscheidungen: [Ticket #1](https://github.com/m324-ap23a-team3/team/issues/1)
- Zeitlogging: _Link folgt_
- Board: _Link folgt_
- Miro: <https://miro.com/app/board/uXjVEQDH4SM=/>
- Auftrag Praxis: [Praxis/Readme.md](https://gitlab.com/ch-tbz-it/Stud/m324/-/blob/nuy/Praxis/Readme.md)
