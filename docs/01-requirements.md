# 01 Anforderungsanalyse

Stand: 2026-11-25 · Status: Freigegeben (2026-11-25) · Paket 1 und 1b

## 1. Ziel und Scope

Eine zentrale Übersicht und Bedienung (Single Pane of Glass) für Gebäudeautomation, Sicherheit und Energie, mit rollenbasiertem Zugriff für Familie und Gäste.

In Scope (Paket 1): Anforderungen, Rollen, Risiken, Frontend-Entscheidung, Kontextdiagramm.
Nicht in Scope: Implementierung, Deployment, Konfigurationsänderungen am Bestand.

## 2. Systeme

| System | Rolle |
|---|---|
| Home Assistant (Container in Portainer, Raspberry Pi 5, 16 GB) | Master für Orchestrierung und Visualisierung, läuft neben anderen Anwendungen (ADR-003) |
| Loxone Miniserver **Gen1** | Gebäudeautomation (Licht, Beschattung, Klima) |
| Acre SPC | Alarmanlage (Sicherheitsebene) |
| Fronius | Energie (zunächst nur Anzeige) |
| Lenovo-Tablets 12,1" | Kiosk-Frontend |
| HomeKit | Alternatives Frontend (offen) |
| Synology NAS | Backup-Ziel |
| GitHub (privat) | Single Source of Truth |

## 3. Rollen

Admin, Eltern, Kinder (Rechte je Kind konfigurierbar), Gast.

## 4. Funktionale Anforderungen

| ID | Anforderung |
|---|---|
| FR-01 | Zentrale Übersicht über Licht, Beschattung, Klima, Alarm-Status und Energie |
| FR-02 | Steuern von Licht, Beschattung und Klima über Loxone |
| FR-03 | SPC: Status anzeigen und scharf/unscharf schalten, Schalten nur mit PIN |
| FR-04 | Fronius: nur Anzeige |
| FR-05 | Rollenbasierter Zugriff für Admin, Eltern, Kinder, Gäste |
| FR-06 | Kinder-Rechte konfigurierbar pro Kind, Raum und Kategorie (z. B. eigenes Zimmer, Wohnzimmer, Playroom) |
| FR-07 | Zeitfenster für Rechte und Automatik (nachts dimmen bzw. aus) |
| FR-08 | Automatik gemäß Kalender. Kalenderquelle ist der interne HA-Kalender (ADR-002) |
| FR-09 | Gäste nutzen nur die Tablets, Zugang per Gäste-Code mit Gültigkeit von–bis, gesteuert über den Kalender |
| FR-10 | HomeKit als Alternative nur für Licht, Beschattung, Klima (Umfang tbd) |
| FR-11 | Fernzugriff ausschließlich über WireGuard-VPN |
| FR-12 | Backup der HA-Konfiguration auf das NAS, Restore getestet |
| FR-13 | Später: Ladestrom-Balancing (Hausbatterie, Auto-Batterie). Jetzt nur Erweiterbarkeit sichern |
| FR-14 | Bidirektionale Schnittstelle zu Loxone Gen1 als Eigenbau nach vorhandener API-Beschreibung (ADR-004) |

## 5. Rechte-Grobmatrix (vorläufig, Details in Paket 2)

| Funktion | Admin | Eltern | Kind | Gast |
|---|---|---|---|---|
| Licht, Beschattung, Klima | ja | ja | nur freigegebene Räume und Zeitfenster | nur Gästebereich |
| Alarm-Status und Rückmeldung sehen | ja | ja | nur ältere Kinder (je Kind freigegeben) | nein |
| Alarm scharf/unscharf (PIN) | ja | ja | ja, mit PIN, wenn je Kind freigegeben (ältere Kinder) | nein |
| Energie-Anzeige | ja (Annahme) | ja (Annahme) | offen (Annahme: nein) | nein |
| Systemverwaltung | ja | nein | nein | nein |

## 6. Nichtfunktionale Anforderungen

| ID | Anforderung |
|---|---|
| NFR-01 | Loxone und SPC arbeiten bei Ausfall von HA unverändert weiter |
| NFR-02 | HA greift mit dedizierten Konten und minimalen Rechten auf Loxone, SPC und Fronius zu |
| NFR-03 | Keine Exposition ins Internet, Zugriff nur per VPN |
| NFR-04 | Secrets liegen nicht im Repository |
| NFR-05 | Konfiguration ist versioniert und reproduzierbar |
| NFR-06 | Backup und Restore sind dokumentiert und getestet |
| NFR-07 | Jede Änderung hat eine Change-Beschreibung, jede wesentliche Entscheidung einen ADR |

## 7. Annahmen

1. Das Netz ist per VLAN segmentiert, Details folgen in Paket 4.
2. Das NAS ist Backup-Ziel, keine Betriebsplattform.
3. Alarm-Schalten aus HA erfolgt über ein restriktives SPC-Konto mit Protokollierung.

## 8. Offene Punkte

| ID | Frage | Standardannahme |
|---|---|---|
| OP-01 | Erledigt (2026-11-25): Ältere Kinder dürfen mit PIN schalten und sehen Status und Rückmeldung, je Kind freigegeben | – |
| OP-02 | Erledigt (2026-11-25): Gäste-Code mit Gültigkeit von–bis, gesteuert über den Kalender | – |
| OP-03 | Erledigt (2026-11-25): Interner HA-Kalender (ADR-002). Funktionsumfang wird in Paket 2 geprüft | – |
| OP-04 | Entschieden (2026-11-25): Bidirektionale Schnittstelle wird gebaut (ADR-004). Offen: API-Beschreibung und Firmware-Version des Miniservers als Eingang für Paket 3 bereitstellen (ohne Zugangsdaten) | Prüfvorbehalt zu vorhandenen Lösungen in Paket 3 |
| OP-05 | HomeKit-Umfang und Gästezugang | tbd |
| OP-06 | Erledigt (2026-11-25): HA bleibt als Container in Portainer, weil weitere Anwendungen auf dem Pi laufen (ADR-003). Der bestehende Container bleibt bis zur Freigabe unverändert | – |
| OP-07 | Wie wird der Gäste-Code erzeugt und geprüft? Home Assistant sieht keine Code-Anmeldung für Gäste nativ vor (zu prüfen) | Klärung in Paket 2 |

## 9. Risiken

| ID | Risiko | Gegenmaßnahme |
|---|---|---|
| R-01 | Home Assistant kennt nativ im Wesentlichen nur Admin und Nicht-Admin. Rechte pro Kind, Raum und Zeit sind nicht eingebaut, und ausgeblendete Dashboards sind keine Sicherheitsgrenze | Eigener ADR in Paket 2, Prüfung von Optionen vor der Umsetzung |
| R-02 | Loxone Gen1 ist ein Altgerät, Schnittstellen und Support sind begrenzt | Prüfung in Paket 3, Fallback-Optionen |
| R-03 | Alarm-Schalten aus HA vergrößert die Angriffsfläche | PIN, Rollenbeschränkung, Audit-Log, VPN-only |
| R-04 | Ausfall des Pi (Speichermedium) | Backup auf NAS, Restore-Test, DR-Plan in Paket 5 |
| R-05 | Fehlerhafte Kalenderautomatik schaltet Licht oder Beschattung unerwünscht | Manuelle Übersteuerung, Tests vor Freigabe |
| R-06 | Kiosk-Tablets lassen sich umgehen oder gestohlen werden | Eigene Kiosk-Konten mit minimalen Rechten, Härtung in Paket 4 |
| R-07 | Der Funktionsumfang des HA-Kalenders reicht für Gäste-Code und Automatik nicht aus | Prüfung in Paket 2, bei Bedarf neuer ADR (Option NAS-Kalender oder Eigenentwicklung) |
| R-08 | Der Pi teilt sich Ressourcen und Netz mit anderen Containern, HA ist kein dedizierter Host | Netztrennung, Ressourcenlimits, getrennte Datenablage in Paket 4 (ADR-003) |
| R-09 | Eigenbau der Loxone-Schnittstelle: Pflegeaufwand, Fehlverhalten kann Gebäudefunktionen beeinflussen | Spezifikation und Tests in Paket 3, Prüfung vorhandener Lösungen, Abschalten ohne Folgen für Loxone (NFR-01) |

## 10. Abnahme von Paket 1

Review der Anforderungen, der Annahmen und der offenen Punkte durch den Auftraggeber. Erst danach starten Paket 2 und 3.
