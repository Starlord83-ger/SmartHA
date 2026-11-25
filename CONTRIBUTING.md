# CONTRIBUTING.md: Arbeitsweise und Release-Management

## Branches

- `main` ist geschützt und enthält nur freigegebene Stände.
- Arbeit in Branches: `paket-<n>/<thema>`, `adr/<nr>-<thema>`, `fix/<thema>`.

## Commits

Titel nach dem Muster `<typ>(<bereich>): <Kurzbeschreibung>` mit Typ `docs`, `adr`, `chg`, `fix` oder `chore`.
Jeder Commit und jeder Push enthält eine Beschreibung mit drei Teilen:

- **Neu:** …
- **Geändert:** …
- **Entfernt:** …

Nicht zutreffende Teile werden mit „nichts“ gefüllt.

## Pull Requests

Jede Änderung geht über einen Pull Request mit Vorlage (`.github/pull_request_template.md`). Merge nur nach Review und Freigabe des Betreibers.

## Releases

- Jedes freigegebene Paket wird getaggt, z. B. `v0.1.0` für die gemeinsam freigegebenen Pakete 1 und 1b. Solange nur Dokumentation existiert, bleibt die Hauptversion 0.
- Der Eintrag im `CHANGELOG.md` und der Paketstatus werden vor dem Tag aktualisiert.
- Der Release-Plan folgt in Paket 6. Bis dahin gilt dieses Vorgehen.

## Definition of Done

Dokumentiert, reviewt, Risiken und Rollback benannt, Teststrategie beschrieben, ADRs angelegt, Index, Log, Changelog und Paketstatus aktualisiert, offene Punkte gelistet, vom Betreiber freigegeben.

## Produktive Änderungen

Nie ohne ausdrückliche Freigabe und dokumentierte Begründung. Siehe `SECURITY.md`.
