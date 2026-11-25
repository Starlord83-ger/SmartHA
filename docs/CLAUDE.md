# docs/CLAUDE.md: Regeln für Dokumente

Gilt zusätzlich zur `CLAUDE.md` im Repo-Root, sobald in `docs/` gearbeitet wird.

## Kopf jedes Dokuments

Titel, dann eine Zeile `Stand: <Datum> · Status: <Entwurf|Akzeptiert|…> · Paket <n>`.

## Pflichtinhalte

- **ADR:** Kontext, Optionen (mit Vor- und Nachteilen), Entscheidung, **Warum**, Konsequenzen, Risiken, Rollback. Vorlage: `adr/ADR-000-template.md`.
- **CHG:** Enthalten (Neu, Geändert, Entfernt), Purpose, Business value, Risks, Dependencies, Rollback, Test strategy. Vorlage: `changes/CHG-000-template.md`.
- **Empfehlungen:** Was, Warum, Nutzen, Nachteile, Risiken, Alternativen.
- **Annahmen** sind als Annahme markiert, offene Fragen als `OP-xx` in `01-requirements.md`.

## Pflege

- `PROJECT_LOG.md` nur anhängen, nie rückwirkend ändern. Korrekturen als neuer Eintrag.
- Ein ADR wird nie gelöscht. Wird er überholt, bekommt er den Status „Ersetzt durch ADR-NNN“.
- Der Index `adr/README.md` und der Paketstatus in `03-development-packages.md` werden bei jeder Änderung mitgeführt.
- `operator-guide.md` wird mit jedem Release ergänzt. Ein Feature gilt erst als fertig, wenn seine Betreiberbeschreibung auf „verfügbar“ steht.

## Stil

Kurz, klar, Tabellen für Vergleiche. Keine Zugangsdaten, keine internen IP-Adressen oder Hostnamen, die nicht für das Verständnis nötig sind.
