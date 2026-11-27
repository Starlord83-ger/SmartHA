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

---

## 2026-11-25: Paket 2 (RBAC-Konzept) erstellt

- **Antworten des Auftraggebers:** Zwei Kinder, alle gleichberechtigt, keine Altersstufen. Zeitfenster sperren nicht, sondern setzen ein anderes Rechtelimit (Profile). Admin und Eltern dürfen jederzeit übersteuern. Alarm-PIN pro Person. Gäste-Code neu pro Besuch. Kalender nur durch Admin und Eltern bearbeitbar.
- **Recherche (Stand 2026-11-25):** HA trennt nativ nur Admin und Nicht-Admin, Ausblenden von Dashboards ist keine Sicherheitsgrenze. Der lokale Kalender ist für Automatisierungen gedacht, nicht als vollwertiger Personenkalender. Das schwächt den ursprünglichen Wunsch nach einer Google-ähnlichen Bedienung (siehe ADR-002, R-07).
- **Entscheidungen (Entwurf, Freigabe offen):**
  - ADR-005: Schutzstufen-Modell. Alarm wird an der Quelle (SPC) abgesichert, Komfortfunktionen für Kinder und Gäste sind Komfortgrenzen. Warum: Der Schaden bei Missbrauch ist beim Alarm hoch, bei Licht und Klima gering, und Eigenentwicklung vermeidet dauerhaften Pflegeaufwand.
  - ADR-006: Gäste-Code als Code-Sperre auf dem Gäste-Dashboard, Besuch wird erst nach Freigabe durch Eltern aktiv. Warum: Der Kalender ist nicht gegen Bearbeitung durch Kinder schützbar.
- **Offen:** OP-07 (Mechanismus), OP-08 (Alarm- und Energie-Rechte der Kinder), OP-09 (Nacht-Profil), OP-10 (Akzeptanz Komfortgrenze). SPC-seitige PIN zu prüfen in Paket 3.
- **Verweise:** `04-rbac.md`, `adr/ADR-005`, `adr/ADR-006`, `changes/CHG-005`.

---

## 2026-11-25: Entscheidungen zu Paket 2

- **Entscheidungen des Auftraggebers:**
  - OP-08: Alarm für Kinder aus, bis Admin ihn freigibt. Energie-Anzeige für Kinder: ja.
  - OP-09: Vorschlag für das Nacht-Profil bestätigt, konkrete Werte legt der Betreiber bei der Umsetzung fest.
  - OP-10: Komfortgrenzen bei Licht, Beschattung und Klima akzeptiert, Alarm wird hart abgesichert.
  - ADR-005: Empfehlung (Schutzstufen-Modell) bestätigt, Status „Akzeptiert“. Warum: Der Aufwand für harte Durchsetzung konzentriert sich auf den Alarm, bei Komfortfunktionen ist der mögliche Schaden gering.
- **Offen:** ADR-006 (Gäste-Code) ist noch nicht entschieden, damit auch OP-07. SPC-seitige PIN zu prüfen in Paket 3.
- **Verweise:** `04-rbac.md`, `adr/ADR-005`, `changes/CHG-005`.

---

## 2026-11-25: Freigabe von Paket 2

- **Entscheidung des Auftraggebers:** ADR-006 (Gäste-Code) bestätigt, Status „Akzeptiert“. PR 3 gemergt. Paket 2 ist damit freigegeben.
- **Warum ADR-006 so:** Option A (Code-Sperre auf dem Gäste-Dashboard, neu pro Besuch, Besuch wird erst nach Freigabe durch Eltern aktiv) ist mit vorhandenen Mitteln machbar und deckt ab, dass der Kalender nicht gegen Bearbeitung durch Kinder schützbar ist.
- **Offen:** Machbarkeit der Code-Sperre und der automatischen Erneuerung vor der Umsetzung prüfen. SPC-seitige PIN prüfen (Paket 3). Releases `v0.1.0` und `v0.2.0` müssen auf GitHub manuell angelegt werden, weil der Tag-Upload aus der Sitzung nicht möglich war.
- **Nächster Schritt:** Paket 3 (Schnittstellen). Eingang: Loxone-API-Beschreibung und Firmware-Version des Miniservers, ohne Zugangsdaten.
- **Verweise:** `adr/ADR-006`, `changes/CHG-006`.

---

## 2026-11-26: Paket 3 (Schnittstellen) als Entwurf erstellt

- **Eingang des Auftraggebers:** Loxone wird gebaut, Spezifikation vorhanden. Fronius nutzt die vorhandene HA-Integration. SPC: FlexC-Spezifikation und Zugang zur HA-Integration vorhanden, Eigenbau oder Integration möglich (Weg nicht festgelegt). Loxone-Firmware-Version noch offen. Geräteliste der Fronius-Anlage geliefert (Anlagenname und Seriennummern bewusst nicht dokumentiert).
- **Recherche (Stand 2026-11-26):** SPC-Anbindung der HA-Core-Integration läuft über ein Drittanbieter-Gateway, laut Forenbericht ohne Code beim Schalten. FlexC ab Panel-Firmware 3.6. Fronius-Integration von HA Core ist lokal und unterstützt Primo, Symo, Symo Hybrid, Zähler und Storage. Loxone Gen1 erhält Updates bis auf Weiteres.
- **Entscheidungen:** ADR-004 Prüfvorbehalt aufgehoben (Auftraggeber bestätigt Eigenbau). ADR-008 Fronius akzeptiert. ADR-007 SPC als Entwurf mit Empfehlung Option D: Anzeige über die HA-Integration, Schalten erst nach Prüfung der FlexC-Spezifikation (OP-13). Warum: Das Schalten ist sicherheitskritisch, das Anzeigen nicht, und Eigenbau bindet dauerhaft Pflegeaufwand.
- **Offen:** OP-11 bis OP-14, Entscheidung zu ADR-007, Freigabe von Paket 3. Releases `v0.1.0` und `v0.2.0` müssen manuell auf GitHub angelegt werden (Anlegen aus der Sitzung ist nicht erlaubt, 403).
- **Verweise:** `05-interfaces.md`, `adr/ADR-007`, `adr/ADR-008`, `changes/CHG-007`.

---

## 2026-11-27: Loxone-Firmware-Version ergänzt

- **Eingang:** Screenshot der Loxone-App. Firmware 17.3.11.10, interne Verbindung „nicht verschlüsselt“. Seriennummer, IP, Benutzername und Anlagenname werden bewusst nicht dokumentiert.
- **Entscheidung:** Keine neue. OP-11 ist teilweise erledigt. Warum: Die Firmware-Hauptversion passt zur öffentlichen API-Beschreibung (V17.0), die Unverschlüsselung verstärkt die Vorgabe „nur Haus-LAN, dediziertes Konto“.
- **Offen:** Abgleich der privaten API-Beschreibung, Verschlüsselungsmöglichkeit auf Gen1.
- **Verweise:** `05-interfaces.md` Abschnitt 2, `01-requirements.md` OP-11.
