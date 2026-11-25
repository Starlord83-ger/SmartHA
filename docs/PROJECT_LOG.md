# Projektlog

Fortlaufende Projektdokumentation. Nur anhängen, nie rückwirkend ändern. Neueste Einträge stehen unten. Korrekturen kommen als neuer Eintrag.

Aufbau je Eintrag: Was, Warum, Entscheidungen, Offen, Verweise.

---

## 2026-11-24: Projektauftrag

- **Was:** Rolle Lead Architect und Senior Developer festgelegt. Zielbild: Home Assistant als Master auf Raspberry Pi 5, Loxone, Acre SPC, Fronius als Ebenen, HomeKit als Familien-Frontend, Lenovo-Tablets im Kiosk-Modus, GitHub als Quelle der Wahrheit. Regelwerk (kein Code ohne Auftrag, ADRs, Änderungsangaben, kein Deployment ohne Freigabe) übernommen.
- **Warum:** Security, Stabilität und Wartbarkeit haben Vorrang vor Features.
- **Offen:** Anforderungen, Rollen, Umfang.

## 2026-11-25: Start mit Dokumentation (Option A)

- **Was:** Verständnis, Annahmen und Klärungsfragen formuliert. Drei Startoptionen bewertet.
- **Entscheidung:** Option A, zuerst Anforderungen und ADRs, noch kein Aufbau.
- **Warum:** Passt zur Priorität Security vor Features und vermeidet Rückbauten. Geringes Risiko, da nichts produktiv geändert wird.
- **Verweise:** `01-requirements.md`.

## 2026-11-25: Klärung der Anforderungen

- **Was:** Antworten des Auftraggebers zu HomeKit-Umfang, Rollen, Ausfallverhalten, Repository und Frontend.
- **Entscheidungen:**
  - HomeKit nur für Licht, Beschattung, Klima. Die Alarmanlage bleibt außerhalb. Warum: klare Sicherheitsgrenze.
  - Rollen Admin, Eltern, Kinder (konfigurierbar pro Kind, Raum, Kategorie, Zeitfenster), Gast.
  - Loxone und SPC laufen bei HA-Ausfall weiter (NFR-01).
  - Neues privates GitHub-Repository.
  - HA ist primäres Frontend, HomeKit nur alternativ (ADR-001).
  - Alarm-Schalten nur mit PIN, Fronius nur Anzeige (später Ladestrom-Balancing), Fernzugriff nur WireGuard.
- **Verweise:** `adr/ADR-001`, `01-requirements.md`.

## 2026-11-25: Paket 1 erstellt

- **Was:** README, Anforderungsanalyse, Kontextdiagramm, ADR-001, CHG-001.
- **Hinweis:** Risiko R-01 festgehalten: HA bietet Rechte pro Kind, Raum und Zeit nicht nativ.
- **Verweise:** `changes/CHG-001`.

## 2026-11-25: Klärung offener Punkte (OP-01 bis OP-03)

- **Entscheidungen:**
  - Ältere Kinder dürfen mit PIN schalten und sehen Status und Rückmeldung, je Kind freigegeben.
  - Gäste-Code mit Gültigkeit von–bis, gesteuert über den Kalender.
  - Es gibt keinen Kalender, ursprünglich war eine Eigenentwicklung gewünscht.
- **Verweise:** `changes/CHG-002`.

## 2026-11-25: Entscheidungen zu Kalender, HA-Betrieb, Loxone

- **Entscheidungen:**
  - Interner HA-Kalender (ADR-002). Warum: kleinste Angriffsfläche, geringster Pflegeaufwand.
  - HA bleibt als Container in Portainer (ADR-003). Warum: weitere Anwendungen laufen auf dem Pi.
  - Loxone Gen1: bidirektionale Schnittstelle als Eigenbau nach vorhandener API-Beschreibung (ADR-004). Warum: Vorgabe des Auftraggebers. Prüfvorbehalt in Paket 3.
- **Beschlossen:** alle empfohlenen Repo-Dateien, Claude-Konfiguration (Sperren, Befehle), Paketkatalog, fortlaufende Projektdokumentation und Operatoranleitung anlegen.
- **Offen:** OP-04 (API-Beschreibung und Firmware-Version bereitstellen), OP-05 (HomeKit), OP-07 (Mechanismus Gäste-Code).
- **Verweise:** `adr/ADR-002` bis `ADR-004`, `03-development-packages.md`, `changes/CHG-003`.

---

## 2026-11-25: Freigabe von Paket 1 und 1b, Import ins Repository

- **Was:** Erst-Import aller Dokumente in das private Repository (Commit auf `main`, weil das Repo bis dahin nur eine Platzhalter-README enthielt). Auftraggeber hat Paket 1 und 1b freigegeben. Status in den Dokumenten auf „freigegeben“ gesetzt, Changelog-Version 0.1.0 vorbereitet.
- **Warum Erst-Import auf `main`:** Das Repo war leer, ein Branch hätte keinen Mehrwert gebracht. Ab jetzt gilt Branch und Pull Request (`CONTRIBUTING.md`).
- **Entscheidung:** Pakete 1 und 1b werden gemeinsam als Release `v0.1.0` getaggt, weil sie in einem Commit importiert wurden.
- **Offen:** Sperrregeln in `.claude/settings.json` mit `/permissions` prüfen (Testpunkt aus CHG-003). Repo-Beschreibung auf GitHub setzen (durch den Betreiber). OP-04, OP-05, OP-07.
- **Verweise:** `changes/CHG-004`, `03-development-packages.md`.
