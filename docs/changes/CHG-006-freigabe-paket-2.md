# CHG-006: Freigabe von Paket 2

Datum: 2026-11-25

## Enthalten

Neu: dieses Dokument.
Geändert: `docs/adr/ADR-006-gaeste-code.md` (Status Akzeptiert), `docs/adr/README.md`, `docs/01-requirements.md` (OP-07 erledigt, Status), `docs/04-rbac.md` (Status, OP-07), `docs/03-development-packages.md` (P2 freigegeben), `README.md`, `docs/operator-guide.md`, `CHANGELOG.md` (Version 0.2.0), `docs/PROJECT_LOG.md`.
Entfernt: nichts.

## Purpose

Entscheidung zu ADR-006 und die Freigabe von Paket 2 dokumentieren.

## Business value

Das Rechtemodell ist vollständig entschieden. Paket 3 kann auf einem festen Stand aufsetzen.

## Risks

Gering, nur Dokumentation. Offen bleibt die Machbarkeit der Code-Sperre und der SPC-seitigen PIN.

## Dependencies

Merge des Pull Requests, danach Release `v0.2.0` auf GitHub.

## Rollback

Git-Revert des Commits. Am produktiven System wurde nichts verändert.

## Test strategy

Abgleich der Statusangaben mit der Entscheidung des Auftraggebers, Prüfung der Verweise.
