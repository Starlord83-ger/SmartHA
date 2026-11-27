# Changelog

Format angelehnt an „Keep a Changelog“. Versionen werden beim Release vergeben.

## [Unreleased]

### Paket 3: Schnittstellen (Entwurf)

**Neu**
- `docs/05-interfaces.md`, `docs/adr/ADR-007-spc-anbindung.md` (Entwurf), `docs/adr/ADR-008-fronius-integration.md`, `docs/changes/CHG-007-paket-3-schnittstellen.md`

**Geändert**
- `docs/adr/ADR-004-loxone-schnittstelle.md` (Prüfvorbehalt aufgehoben), Firmware-Version in `docs/05-interfaces.md` und OP-11, `docs/adr/README.md`, `docs/01-requirements.md` (OP-04, OP-11 bis OP-14, R-12, R-13), `docs/02-architecture-context.md`, `docs/04-rbac.md`, `docs/03-development-packages.md`, `docs/operator-guide.md`, `README.md`, `docs/PROJECT_LOG.md`

**Entfernt**
- nichts

## [0.2.0] - 2026-11-25 (Paket 2, freigegeben; enthält auch die Doku der Freigabe von Paket 1 und 1b)

Zusätzlich zu den folgenden Einträgen: ADR-006 akzeptiert, OP-07 erledigt, Status von Paket 2 auf „freigegeben“ (`docs/changes/CHG-006-freigabe-paket-2.md`).

### Paket 2: RBAC-Konzept (Entwurf)

**Neu**
- `docs/04-rbac.md`, `docs/adr/ADR-005-rechteumsetzung.md`, `docs/adr/ADR-006-gaeste-code.md`, `docs/changes/CHG-005-paket-2-rbac.md`

**Geändert**
- `docs/01-requirements.md` (OP-01, OP-07 bis OP-10, R-01, R-10, R-11, FR-07, FR-09, FR-15, Rechtematrix)
- `docs/adr/README.md`, `docs/03-development-packages.md`, `docs/operator-guide.md`, `README.md`, `docs/PROJECT_LOG.md`
- ADR-005 auf „Akzeptiert“, OP-08 bis OP-10 entschieden

**Entfernt**
- nichts

### Freigabe von Paket 1 und 1b

**Neu**
- `docs/changes/CHG-004-freigabe-paket-1-1b.md`

**Geändert**
- Status auf „freigegeben“ in `README.md`, `docs/01-requirements.md`, `docs/02-architecture-context.md`, `docs/03-development-packages.md`
- `CONTRIBUTING.md`: Beispiel zum Release-Tag
- `docs/PROJECT_LOG.md`: neuer Eintrag

**Entfernt**
- nichts

## [0.1.0] - 2026-11-25 (Pakete 1 und 1b, gemeinsam freigegeben)

### Paket 1b: Repo-Standards und Entscheidungen

**Neu**
- `CLAUDE.md`, `docs/CLAUDE.md`, `SECURITY.md`, `CONTRIBUTING.md`, `CHANGELOG.md`, `.gitignore`, `.github/pull_request_template.md`
- `.claude/settings.json` und Befehle `/neuer-adr`, `/change-beschreibung`, `/paket-abschluss`
- ADR-000 (Vorlage), ADR-002 (Kalender), ADR-003 (HA als Container), ADR-004 (Loxone-Schnittstelle), ADR-Index
- `docs/03-development-packages.md`, `docs/PROJECT_LOG.md`, `docs/operator-guide.md`, `docs/runbooks/` (Gerüste), `docs/changes/CHG-000` bis `CHG-003`
- `docs/claude-project-instructions.md`

**Geändert**
- `docs/01-requirements.md`: OP-03, OP-04, OP-06 entschieden, neue OP-07, R-07 bis R-09, FR-14
- `docs/02-architecture-context.md`: Beschriftung HA und Loxone
- `README.md`: Struktur und Roadmap

**Entfernt**
- nichts

### Paket 1: Grundlagen

**Neu**
- `README.md`, `docs/01-requirements.md`, `docs/02-architecture-context.md`, `docs/adr/ADR-001-frontend-strategie.md`, `docs/changes/CHG-001` und `CHG-002`

**Geändert**
- nichts

**Entfernt**
- nichts
