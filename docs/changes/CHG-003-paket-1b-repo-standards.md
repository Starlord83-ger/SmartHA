# CHG-003: Paket 1b, Repo-Standards und Entscheidungen

Datum: 2026-11-25

## Enthalten

Neu:
- `CLAUDE.md`, `docs/CLAUDE.md`, `SECURITY.md`, `CONTRIBUTING.md`, `CHANGELOG.md`, `.gitignore`, `.github/pull_request_template.md`
- `.claude/settings.json`, `.claude/commands/` (`neuer-adr`, `change-beschreibung`, `paket-abschluss`)
- `docs/adr/README.md`, `ADR-000-template.md`, `ADR-002`, `ADR-003`, `ADR-004`
- `docs/03-development-packages.md`, `docs/PROJECT_LOG.md`, `docs/operator-guide.md`
- `docs/runbooks/update.md`, `docs/runbooks/restore.md`
- `docs/changes/CHG-000-template.md`, dieses Dokument
- `docs/claude-project-instructions.md`

Geändert:
- `docs/01-requirements.md` (OP-03, OP-04, OP-06 entschieden, neue OP-07, R-07 bis R-09, FR-14, Systemtabelle)
- `docs/02-architecture-context.md` (Beschriftung HA und Loxone)
- `README.md` (Struktur, Roadmap)

Entfernt: nichts.

## Purpose

Nachvollziehbarkeit von Anfang an sichern (Log, ADRs mit „Warum“, Paketkatalog) und die inzwischen gefallenen Entscheidungen festhalten.

## Business value

Entscheidungen und ihre Gründe bleiben auch später nachvollziehbar. Claude arbeitet in jeder Sitzung nach denselben Regeln. Versehentliche Zugriffe auf Geheimnisse oder Deployments werden technisch gebremst.

## Risks

- Prozessaufwand für ein Privatprojekt. Vorlagen und Befehle begrenzen ihn.
- Die Sperrregeln in `.claude/settings.json` sind in der Syntax nicht geprüft und müssen nach dem ersten Start mit `/permissions` kontrolliert werden.
- Der Operator-Guide beschreibt nur den Soll-Zustand, bis Features umgesetzt sind.

## Dependencies

ADR-002 bis ADR-004 setzen Entscheidungen des Auftraggebers um. Für P3 werden die Loxone-API-Beschreibung und die Firmware-Version benötigt.

## Rollback

Git-Revert des Commits. Am produktiven System wurde nichts verändert, der bestehende HA-Container blieb unberührt.

## Test strategy

Inhaltlicher Review, Gültigkeit der JSON-Datei, Prüfung der Verweise zwischen Dokumenten, Prüfung der Sperrregeln mit `/permissions` nach der Übernahme ins Repository.
