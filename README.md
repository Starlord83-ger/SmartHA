# Home Operations Platform

Privates Repository für Architektur, Dokumentation und später die versionierte Konfiguration der Home Operations Platform: Home Assistant (Container auf einem Raspberry Pi 5, neben anderen Diensten) als zentrale Oberfläche über Loxone (Gebäude), Acre SPC (Sicherheit) und Fronius (Energie). Bedienung über Kiosk-Tablets, Zugriff von außen nur per WireGuard-VPN.

**Status:** Phase Dokumentation. Es gibt noch keinen Code und kein Deployment.

## Leitprinzipien

Priorität: Security > Stabilität > Wartbarkeit > Performance > Features.

- Loxone und SPC bleiben eigenständig und funktionieren auch ohne Home Assistant.
- Least Privilege und Secure-by-default.
- GitHub ist die einzige Quelle der Wahrheit. Secrets gehören nie ins Repo.
- Jede wesentliche Entscheidung bekommt einen ADR, jede Änderung eine Change-Beschreibung.
- Kein produktives Deployment und keine Konfigurationsänderung ohne ausdrückliche Freigabe.

## Struktur

| Pfad | Inhalt |
|---|---|
| `docs/01-requirements.md` | Anforderungsanalyse, Rollen, Risiken, offene Punkte |
| `docs/02-architecture-context.md` | Kontextdiagramm (Mermaid) |
| `docs/adr/` | Architecture Decision Records |
| `docs/changes/` | Change-Beschreibungen (Zweck, Nutzen, Risiken, Abhängigkeiten, Rollback, Test) |
| `docs/03-development-packages.md` | Katalog aller Entwicklungspakete mit Inhalt, Begründung und Status |
| `docs/04-rbac.md` | RBAC-Konzept: Rollen, Schutzstufen, Rechtematrix, Gäste-Code |
| `docs/PROJECT_LOG.md` | Fortlaufende Projektdokumentation (nur anhängen) |
| `docs/operator-guide.md` | Operatoranleitung (Features konfigurieren und anwenden) |
| `docs/runbooks/` | Betriebsanleitungen für Update und Restore |
| `docs/claude-project-instructions.md` | Projektanweisung für Claude (zum Einfügen ins Claude-Projekt) |
| `CLAUDE.md`, `docs/CLAUDE.md` | Arbeitsregeln für Claude im Repo |
| `SECURITY.md`, `CONTRIBUTING.md`, `CHANGELOG.md` | Sicherheit, Arbeitsweise und Release-Management, Änderungshistorie |
| `.claude/` | Sperren und Befehle für Claude Code (Werkzeugkonfiguration) |
| `.github/` | Vorlage für Pull Requests |

## Roadmap der Dokumentationspakete

| Paket | Inhalt | Status |
|---|---|---|
| 1 | Requirements, ADR-001 Frontend, Kontextdiagramm | freigegeben |
| 1b | Repo-Standards, ADR-002 bis 004, Paketkatalog, Projektlog, Operatoranleitung | freigegeben |
| 2 | RBAC-Konzept, ADR-005 und ADR-006 | freigegeben |
| 3 | Schnittstellen (Loxone Gen1 Eigenbau, SPC, Fronius) | geplant |
| 4 | Netzwerk, Security, Secrets | geplant |
| 5 | Backup und Disaster-Recovery-Plan | geplant |
| 6 | Use-Case-Katalog, Test-, Release-Plan, Roadmap | geplant |

Details und Begründungen: `docs/03-development-packages.md`.

## Commit-Regel

Jeder Commit und jeder Push enthält eine Beschreibung, was sich geändert hat, was neu ist und was entfernt wurde.
