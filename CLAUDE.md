# CLAUDE.md: Arbeitsanweisung für Claude in diesem Repository

Kurzbeschreibung: Privates Repository für Architektur und Dokumentation der Home Operations Platform. Home Assistant läuft als Container auf einem Raspberry Pi 5 und führt Loxone (Gen1), Acre SPC und Fronius zusammen. Bedienung über Kiosk-Tablets, Fernzugriff nur per WireGuard.

## Phase

Dokumentationsphase. Es gibt keinen Implementierungscode und kein Deployment. Die Dateien in `.claude/` sind Werkzeugkonfiguration, kein Produktcode.

## Zuerst lesen (in dieser Reihenfolge)

1. `docs/PROJECT_LOG.md` (letzte Einträge unten)
2. `docs/03-development-packages.md` (Paketstatus)
3. `docs/01-requirements.md` (offene Punkte OP-xx, Risiken R-xx)
4. `docs/adr/README.md` (Entscheidungsindex)

## Verbindliche Regeln

1. Kein Implementierungscode, außer er wird ausdrücklich verlangt. Nichts entwickeln, was nicht angefragt wurde. Ideen dürfen mit Begründung vorgeschlagen werden.
2. Kein produktives Deployment und keine Konfigurationsänderung ohne ausdrückliche Freigabe und dokumentierte Begründung. Das gilt auch für den bestehenden HA-Container.
3. Bei komplexen Anfragen zuerst Verifikationsfragen stellen. Zu jeder Frage die Standardannahme nennen.
4. Vor jeder Umsetzung: Verständnis zusammenfassen, Annahmen, Rückfragen, Scope, Alternativen mit Vor- und Nachteilen, Empfehlung.
5. Jede Empfehlung nennt: Was, Warum, Nutzen, Nachteile, Risiken, Alternativen.
6. Prioritäten: Security > Stabilität > Wartbarkeit > Performance > Features.
7. Least Privilege, Secure-by-default. Siehe `SECURITY.md`.
8. Prüfbare Aussagen zu Versionen und Schnittstellen (HA, Loxone Gen1, SPC, Fronius) werden geprüft oder als „zu prüfen“ markiert. Nichts erfinden.

## Nachvollziehbarkeit (Pflicht bei jeder Arbeitssitzung)

- Eintrag in `docs/PROJECT_LOG.md` (nur anhängen, nie umschreiben): Datum, Was, Warum, Entscheidungen, offene Punkte, Verweise.
- Jede wesentliche Entscheidung bekommt einen ADR mit „Warum“. Der Index in `docs/adr/README.md` wird mitgepflegt.
- Jede Änderung bekommt eine `docs/changes/CHG-NNN-*.md` mit Zweck, Business Value, Risiken, Abhängigkeiten, Rollback, Teststrategie.
- Paketstatus in `docs/03-development-packages.md` und `CHANGELOG.md` aktualisieren.
- Jede Lieferung endet mit Commit-Beschreibung: was geändert wurde, was neu ist, was entfernt wurde.

## Konventionen

- Sprache: Deutsch. Datumsformat ISO (2026-11-25).
- IDs: `FR-` (funktional), `NFR-`, `R-` (Risiko), `OP-` (offener Punkt), `ADR-NNN`, `CHG-NNN`. Nummern werden nie wiederverwendet.
- Statuswerte: Entwurf, Akzeptiert, Ersetzt (bei ADRs), geplant, in Arbeit, freigegeben (bei Paketen).
- Diagramme in Mermaid. Orange = bidirektionale Schnittstelle, Türkis = Tablets.
- Bestehende Dokumente pflegen statt Parallelversionen anlegen. Relative Links verwenden.
- Weitere Detailregeln für Dokumente stehen in `docs/CLAUDE.md`.

## Sicherheit

- Keine Secrets in Chat, Repo oder Dokumentation (Passwörter, Tokens, PINs, Gäste-Codes, VPN-Schlüssel). Nur den Ablageort nennen.
- `.claude/settings.json` sperrt Secret-Dateien und riskante Befehle. Diese Sperren nicht umgehen. Nach Änderungen mit `/permissions` prüfen.

## Befehle

- `/neuer-adr <Thema>`: ADR nach Vorlage anlegen und Index pflegen
- `/change-beschreibung <Thema>`: CHG-Datei und Commit-Beschreibung erzeugen
- `/paket-abschluss <Paket>`: Definition of Done prüfen, Status, Log und Changelog aktualisieren

## Pflege dieser Datei

Kurz halten und nur dauerhafte Regeln aufnehmen. Details gehören nach `docs/`. Änderungen an dieser Datei laufen über eine CHG-Datei und einen Logeintrag.
