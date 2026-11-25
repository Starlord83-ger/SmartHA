# CHG-005: Paket 2, RBAC-Konzept

Datum: 2026-11-25

## Enthalten

Neu:
- `docs/04-rbac.md`
- `docs/adr/ADR-005-rechteumsetzung.md`, `docs/adr/ADR-006-gaeste-code.md` (beide Entwurf)
- dieses Dokument

Geändert:
- `docs/01-requirements.md` (OP-01 präzisiert, OP-07 fortgeschrieben, neue OP-08 bis OP-10, R-01 präzisiert, neue R-10 und R-11, FR-07, FR-09, neue FR-15, Rechtematrix)
- `docs/adr/README.md`, `docs/03-development-packages.md`, `docs/operator-guide.md`, `README.md`, `CHANGELOG.md`, `docs/PROJECT_LOG.md`

Entfernt: nichts.

## Purpose

Das Rechtemodell eindeutig festlegen und ehrlich ausweisen, welche Rechte Home Assistant tatsächlich durchsetzen kann.

## Business value

Der Alarm wird an der Stelle abgesichert, an der es zählt. Es entsteht keine trügerische Sicherheit durch ausgeblendete Dashboards. Der Aufwand bleibt klein.

## Risks

Das Restrisiko für Komfortfunktionen (S2) muss der Betreiber akzeptieren (OP-10). Die SPC-seitige PIN ist noch nicht auf Machbarkeit geprüft (Paket 3). Die Recherche stützt sich auf Dokumentation und Community-Beiträge, nicht auf einen Test an der eigenen Installation.

## Dependencies

Entscheidung zu ADR-005 und ADR-006, Antworten zu OP-08 bis OP-10. Paket 3 prüft die SPC-Anbindung.

## Rollback

Git-Revert des Commits. Am produktiven System wurde nichts verändert.

## Test strategy

Inhaltlicher Review der Rechtematrix, Abgleich mit den Antworten des Auftraggebers, Prüfung der Quellenlinks. Technische Prüfungen folgen in Paket 3 und in den Implementierungspaketen.

## Nachtrag 2026-11-25

ADR-005 ist akzeptiert, OP-08 bis OP-10 sind entschieden (Alarm für Kinder aus bis Freigabe, Energie für Kinder ja, Nacht-Profil-Vorschlag und Komfortgrenzen akzeptiert). ADR-006 bleibt Entwurf. Betroffene Dateien: `docs/04-rbac.md`, `docs/01-requirements.md`, `docs/adr/ADR-005-rechteumsetzung.md`, `docs/adr/README.md`, `docs/operator-guide.md`, `docs/03-development-packages.md`, `CHANGELOG.md`, `docs/PROJECT_LOG.md`.
