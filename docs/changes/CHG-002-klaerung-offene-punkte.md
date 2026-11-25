# CHG-002: Klärung offener Punkte aus Paket 1

Datum: 2026-11-25

## Enthalten

Geändert: `docs/01-requirements.md` (Rechte-Matrix, FR-08, FR-09, OP-01 bis OP-03, neue OP-06, neue Risiken R-07 und R-08).
Neu: dieses Dokument.
Entfernt: nichts.

## Purpose

Antworten des Auftraggebers einarbeiten und die dabei entstandenen neuen Entscheidungen sichtbar machen (Kalender, HA-Installationsform).

## Business value

Verhindert, dass Annahmen unbemerkt in die Umsetzung wandern. Der Kalender-Aufwand wird früh sichtbar.

## Risks

Gering, nur Dokumentation. Eine eigene Kalender-Entwicklung ist ein erheblicher Zusatzumfang (R-07).

## Dependencies

Entscheidung zu ADR-002 (Kalender) und ADR-003 (HA-Installationsform).

## Rollback

Git-Revert des Commits. Das produktive System bleibt unverändert.

## Test strategy

Inhaltlicher Review, Abgleich mit den Antworten des Auftraggebers.
