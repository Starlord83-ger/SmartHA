---
description: Neuen ADR nach Vorlage anlegen und den Index pflegen
argument-hint: <Thema>
---

Lege einen neuen Architecture Decision Record zum Thema "$ARGUMENTS" an.

1. Lies `docs/adr/README.md` und ermittle die nächste freie Nummer. Nummern werden nie wiederverwendet.
2. Fasse Verständnis, Annahmen und offene Fragen zusammen. Bei Unklarheiten stelle zuerst Verifikationsfragen.
3. Lege `docs/adr/ADR-NNN-<kurzer-name>.md` nach `docs/adr/ADR-000-template.md` an. Pflicht: Kontext, Optionen mit Vor- und Nachteilen, Entscheidung, **Warum**, Konsequenzen, Risiken, Rollback.
4. Trage den ADR im Index in `docs/adr/README.md` ein.
5. Hänge einen Eintrag an `docs/PROJECT_LOG.md` an.
6. Schreibe keinen Implementierungscode und ändere nichts am produktiven System.
7. Schließe mit der Commit-Beschreibung ab (Neu, Geändert, Entfernt).
