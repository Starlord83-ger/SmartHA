# 03 Entwicklungspakete

Stand: 2026-11-25 · Status: fortlaufend gepflegt

Prinzip: kleine bis mittlere Pakete, jedes mit Review und Freigabe. Die Reihenfolge folgt den Prioritäten Security > Stabilität > Wartbarkeit > Performance > Features. Statuswerte: geplant, in Arbeit, Entwurf (zur Freigabe), freigegeben.

## Übersicht

| Paket | Titel | Status | Hauptergebnisse |
|---|---|---|---|
| P1 | Grundlagen | freigegeben (2026-11-25) | README, Anforderungen, Kontextdiagramm, ADR-001 |
| P1b | Repo-Standards und Entscheidungen | freigegeben (2026-11-25) | CLAUDE.md, Vorlagen, ADR-002 bis 004, Paketkatalog, Projektlog, Operatoranleitung |
| P2 | RBAC-Konzept | freigegeben (2026-11-25) | `04-rbac.md`, ADR-005 (Rechteumsetzung), ADR-006 (Gäste-Code) |
| P3 | Schnittstellen | Entwurf (zur Freigabe) | `05-interfaces.md`, ADR-007, ADR-008 |
| P4 | Netzwerk und Security | geplant | Netz-/Sicherheitskonzept, Secret-Ablage, Härtung |
| P5 | Backup und Disaster Recovery | geplant | DR-Plan, vervollständigte Runbooks |
| P6 | Use Cases, Tests, Release, Roadmap | geplant | Use-Case-Katalog, Testplan, Release-Plan, Roadmap |

## Zuordnung der erwarteten Ergebnisse

| Ergebnis | Paket |
|---|---|
| Anforderungsanalyse | P1 (laufend gepflegt) |
| Architekturdiagramme | P1 (Kontext), P3 (Schnittstellen), P4 (Netz) |
| Schnittstellendokumentation | P3 |
| ADRs | laufend |
| Use-Case-Katalog | P6 |
| RBAC-Dokumentation | P2 |
| Entwicklungs-Roadmap | P6 |
| Release-Plan | P6 |
| Testplan | P6 |
| Disaster-Recovery-Plan | P5 |
| Operatoranleitung | laufend, mit jedem Release ergänzt |
| Fortlaufende Projektdokumentation | laufend (`PROJECT_LOG.md`) |

## Paketbeschreibungen

### P1 Grundlagen

- **Beschreibung:** Gemeinsame Basis aus Zielbild, Rollen, Anforderungen, Risiken und Frontend-Entscheidung.
- **Inhalt:** `README.md`, `01-requirements.md`, `02-architecture-context.md`, `adr/ADR-001`, `changes/CHG-001`, `CHG-002`.
- **Warum zuerst:** Ohne geklärte Anforderungen entstehen Rückbauten. Das passt zur Priorität Security vor Features.
- **Abhängigkeiten:** keine.
- **Risiken:** Falsche Annahmen pflanzen sich fort. Gegenmaßnahme sind die offenen Punkte `OP-xx`.
- **Abnahme:** Review und Freigabe durch den Betreiber.

### P1b Repo-Standards und Entscheidungen

- **Beschreibung:** Regeln für Nachvollziehbarkeit und Zusammenarbeit mit Claude sowie die inzwischen gefallenen Entscheidungen.
- **Inhalt:** `CLAUDE.md`, `docs/CLAUDE.md`, `SECURITY.md`, `CONTRIBUTING.md`, `CHANGELOG.md`, `.gitignore`, PR-Vorlage, `.claude/` (Sperren und drei Befehle), ADR-Vorlage und ADR-002 bis 004, `PROJECT_LOG.md`, `operator-guide.md`, Runbook-Gerüste.
- **Warum vor den Fachpaketen:** Nachvollziehbarkeit muss von Anfang an gelten, nicht nachträglich. Die Entscheidungen zu Kalender, HA-Betrieb und Loxone-Schnittstelle sind Voraussetzung für P2 und P3.
- **Abhängigkeiten:** P1.
- **Risiken:** Zu viel Prozess für ein Privatprojekt. Gegenmaßnahme: Vorlagen und Befehle halten den Aufwand klein.
- **Abnahme:** Review durch den Betreiber, Prüfung der `.claude/settings.json` mit `/permissions`.

### P2 RBAC-Konzept

- **Beschreibung:** Verbindliches Rechtemodell für Admin, Eltern, Kinder und Gäste.
- **Inhalt:** `04-rbac.md` mit Rollen, Rechtematrix je Kind, Raum, Kategorie und Zeitfenster, PIN-Konzept für die Alarmanlage, Mechanismus für den Gäste-Code auf Basis des HA-Kalenders (ADR-002), Rechte zur Kalenderbearbeitung. Neuer ADR zur technischen Umsetzung.
- **Warum nach ADR-002:** Gäste-Code und Zeitfenster hängen am Kalender.
- **Abhängigkeiten:** P1b. Alle zugehörigen offenen Punkte (OP-07 bis OP-10) sind entschieden.
- **Risiken:** R-01. Home Assistant bringt feingranulare Rechte nicht nativ mit.
- **Abnahme:** Jede Zeile der Rechtematrix ist eindeutig und prüfbar.

### P3 Schnittstellen

- **Beschreibung:** Spezifikation aller Anbindungen an Home Assistant.
- **Inhalt:** `05-interfaces.md` mit Loxone Gen1 (Eigenbau nach ADR-004, Datenmodell, Fehlerverhalten, Sicherheit, Tests), Acre SPC (Status, Scharfschalten mit PIN) und Fronius (nur Anzeige, Erweiterbarkeit für Ladestrom-Balancing). Prüfung, ob vorhandene Lösungen den Loxone-Eigenbau verkleinern.
- **Warum nach P2:** Die Schnittstellen müssen die Rechte aus P2 abbilden können.
- **Abhängigkeiten:** P2, Firmware-Version und Abgleich der API-Beschreibung von Loxone (OP-11), FlexC-Prüfung (OP-13).
- **Stand (2026-11-26):** Loxone bleibt Eigenbau (ADR-004), Fronius nutzt die vorhandene HA-Integration (ADR-008), SPC wird über eine eigene FlexC-Engine angebunden (ADR-007, akzeptiert 2026-11-27).
- **Risiken:** R-03, R-09.
- **Abnahme:** Je Schnittstelle sind Richtung, Daten, Rechte, Fehlerfälle und Tests beschrieben.

### P4 Netzwerk und Security

- **Beschreibung:** Technische Absicherung der Plattform.
- **Inhalt:** `06-security-network.md` mit Netzwerkzonen, WireGuard-Zugang, Containerhärtung (HA neben anderen Containern, Netzwerkmodus, Ressourcenlimits), Ablage der Geheimnisse, Kiosk-Härtung, Audit-Protokolle, Secret-Scan.
- **Warum:** Security hat höchste Priorität, und die Ergebnisse beeinflussen den Betrieb.
- **Abhängigkeiten:** P3.
- **Risiken:** R-06, R-08.
- **Abnahme:** Jede Zone und jeder Zugangsweg ist beschrieben und begründet.

### P5 Backup und Disaster Recovery

- **Beschreibung:** Wiederherstellbarkeit der Plattform.
- **Inhalt:** `07-backup-dr.md`, Backup-Verfahren für den HA-Container (nach ADR-003), Ziele für Wiederherstellungszeit und Datenverlust, Restore-Test, vollständige Runbooks für Restore und Update.
- **Warum:** Stabilität kommt vor Features. Ohne Restore-Test ist ein Backup nur eine Annahme.
- **Abhängigkeiten:** P4.
- **Risiken:** R-04.
- **Abnahme:** Ein dokumentierter Restore wurde mit Freigabe getestet.

### P6 Use Cases, Tests, Release, Roadmap

- **Beschreibung:** Abschluss der Planungsphase.
- **Inhalt:** `08-use-cases.md`, `09-test-plan.md`, `10-release-plan.md`, `11-roadmap.md` mit Zuschnitt der Implementierungspakete.
- **Warum zuletzt:** Tests und Releases beziehen sich auf alle vorherigen Ergebnisse.
- **Abhängigkeiten:** P2 bis P5.
- **Abnahme:** Jede Anforderung hat mindestens einen Testfall.

## Implementierungspakete

Erst nach Abschluss von P1 bis P6 und nur mit ausdrücklicher Freigabe. Der Zuschnitt (z. B. HA-Basiskonfiguration, Loxone-Schnittstelle, SPC-Anbindung, Fronius-Anzeige, Dashboards und Kiosk, RBAC-Umsetzung) wird in P6 festgelegt.
