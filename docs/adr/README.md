# Architecture Decision Records

Jede wesentliche Entscheidung bekommt einen ADR mit **Warum**. Vorlage: `ADR-000-template.md`. Nummern werden nie wiederverwendet, ADRs werden nie gelöscht, sondern auf „Ersetzt“ gesetzt.

## Index

| ADR | Titel | Status | Datum |
|---|---|---|---|
| [ADR-001](ADR-001-frontend-strategie.md) | Frontend-Strategie (HA primär, HomeKit alternativ) | Akzeptiert | 2026-11-25 |
| [ADR-002](ADR-002-kalender.md) | Kalender: interner HA-Kalender | Akzeptiert | 2026-11-25 |
| [ADR-003](ADR-003-ha-container.md) | Home Assistant bleibt als Container in Portainer | Akzeptiert | 2026-11-25 |
| [ADR-004](ADR-004-loxone-schnittstelle.md) | Loxone Gen1: bidirektionale Schnittstelle als Eigenbau | Akzeptiert mit Prüfvorbehalt | 2026-11-25 |

## Geplant

| Thema | Paket |
|---|---|
| Umsetzung der Rechte (pro Kind, Raum, Kategorie, Zeit) und des Gäste-Codes | 2 |
| Netzwerkzonen, Container-Härtung, Secret-Ablage | 4 |
| Backup-Verfahren für den HA-Container | 5 |
