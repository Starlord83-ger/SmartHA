# CHG-007: Paket 3, Schnittstellen (Entwurf)

Datum: 2026-11-26

## Enthalten

- Neu: `docs/05-interfaces.md`, `docs/adr/ADR-007-spc-anbindung.md` (Akzeptiert 2026-11-27), `docs/adr/ADR-008-fronius-integration.md` (Akzeptiert), dieses Dokument.
- Geändert: `docs/adr/ADR-004-loxone-schnittstelle.md` (Prüfvorbehalt aufgehoben), `docs/adr/README.md`, `docs/01-requirements.md` (OP-04, OP-11 bis OP-14, R-12, R-13), `docs/02-architecture-context.md` (Verweis), `docs/04-rbac.md` (Verweis Abschnitt 7), `docs/03-development-packages.md`, `docs/operator-guide.md`, `README.md`, `CHANGELOG.md`, `docs/PROJECT_LOG.md`.
- Entfernt: nichts.

## Purpose

Anbindung von Loxone Gen1, SPC und Fronius an HA beschreiben, Entscheidungen festhalten, offene Prüfungen benennen.

## Business value

Die Umsetzung kann je Schnittstelle gezielt freigegeben werden. Fronius ist entschieden, Loxone ist spezifiziert, bei SPC ist klar, wovon die Entscheidung abhängt.

## Risks

- R-13: Eigenbau der FlexC-Engine bindet Pflegeaufwand und vergrößert die Angriffsfläche.
- R-14: Gespeicherte SPC-Zugangsdaten erlauben Schalten, das Panel sieht nur das Dienstkonto.
- R-12 ist gegenstandslos (HA-Integration wird nicht genutzt).
- R-09: Eigenbau Loxone, besteht weiter.
- Aussagen zu Loxone Gen1, SPC und Fronius sind aus öffentlichen Quellen und Forenbeiträgen, nicht am Gerät geprüft.

## Dependencies

Paket 2 (Rechtemodell). Eingang des Auftraggebers: Firmware-Version des Miniservers, Abgleich der privaten Loxone-API-Beschreibung, FlexC-Spezifikation, Prüfung OP-13.

## Rollback

Dokumente zurücksetzen (Revert des Pull Requests). Es gibt keine technische Änderung am Bestand.

## Test strategy

Review durch den Auftraggeber. Prüfbarkeit: Jede Schnittstelle in `05-interfaces.md` hat Richtung, Daten, Rechte, Fehlerfälle und Tests. Alle IDs (OP, R, ADR, CHG) einmalig und im Index vorhanden.
