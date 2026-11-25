---
description: Change-Beschreibung (CHG) und Commit-Beschreibung erzeugen
argument-hint: <Thema>
---

Erstelle die Change-Beschreibung zum Thema "$ARGUMENTS".

1. Ermittle die nächste freie CHG-Nummer in `docs/changes/`.
2. Lege `docs/changes/CHG-NNN-<kurzer-name>.md` nach `docs/changes/CHG-000-template.md` an. Pflicht: Enthalten (Neu, Geändert, Entfernt), Purpose, Business value, Risks, Dependencies, Rollback, Test strategy.
3. Trage die Änderung im `CHANGELOG.md` unter [Unreleased] ein.
4. Hänge einen Eintrag an `docs/PROJECT_LOG.md` an.
5. Gib am Ende die Commit-Beschreibung mit den drei Teilen Neu, Geändert, Entfernt aus.
