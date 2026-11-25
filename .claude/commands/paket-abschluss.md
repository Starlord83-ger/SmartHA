---
description: Definition of Done für ein Paket prüfen und die Doku nachziehen
argument-hint: <Paketnummer>
---

Prüfe den Abschluss von Paket "$ARGUMENTS" gegen die Definition of Done in `CONTRIBUTING.md`.

1. Gehe die Checkliste durch: Dokumentation vollständig, Risiken und Rollback benannt, Teststrategie beschrieben, ADRs mit „Warum“ angelegt, offene Punkte gelistet.
2. Melde jede Lücke einzeln. Behebe nichts, was nicht verlangt wurde.
3. Wenn alles erfüllt ist: Paketstatus in `docs/03-development-packages.md` auf „Entwurf (zur Freigabe)“ setzen, `CHANGELOG.md` und `docs/PROJECT_LOG.md` ergänzen.
4. Setze den Status nie selbst auf „freigegeben“. Das entscheidet der Betreiber.
5. Gib die Commit-Beschreibung (Neu, Geändert, Entfernt) und den Vorschlag für den Release-Tag aus.
