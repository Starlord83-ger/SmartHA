# CHG-004: Freigabe von Paket 1 und 1b

Datum: 2026-11-25

## Enthalten

Neu: dieses Dokument.
Geändert: Statusangaben in `README.md`, `docs/01-requirements.md`, `docs/02-architecture-context.md`, `docs/03-development-packages.md`, Release-Beispiel in `CONTRIBUTING.md`, `CHANGELOG.md` (Version 0.1.0), `docs/PROJECT_LOG.md`.
Entfernt: nichts.

## Purpose

Die Freigabe des Auftraggebers dokumentieren und den freigegebenen Stand als Release `v0.1.0` markierbar machen.

## Business value

Klarer, nachvollziehbarer Stand, auf dem Paket 2 aufbaut.

## Risks

Gering, nur Dokumentation. Offen bleibt die Prüfung der Sperrregeln mit `/permissions`.

## Dependencies

Merge des Pull Requests durch den Betreiber, danach Tag `v0.1.0`.

## Rollback

Git-Revert des Commits. Am produktiven System wurde nichts verändert.

## Test strategy

Inhaltlicher Abgleich der Statusangaben mit der Freigabe, Prüfung der Links.
