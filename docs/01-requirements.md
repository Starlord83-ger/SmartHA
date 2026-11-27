# 01 Anforderungsanalyse

Stand: 2026-11-25 · Status: Freigegeben (Pakete 1, 1b und 2)

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

Admin, Eltern, zwei Kinder (gleichberechtigt, Rechte je Kind konfigurierbar), Gast.

## 4. Funktionale Anforderungen

| ID | Anforderung |
|---|---|
| FR-01 | Zentrale Übersicht über Licht, Beschattung, Klima, Alarm-Status und Energie |
| FR-02 | Steuern von Licht, Beschattung und Klima über Loxone |
| FR-03 | SPC: Status anzeigen und scharf/unscharf schalten, Schalten nur mit PIN |
| FR-04 | Fronius: nur Anzeige |
| FR-05 | Rollenbasierter Zugriff für Admin, Eltern, Kinder, Gäste |
| FR-06 | Kinder-Rechte konfigurierbar pro Kind, Raum und Kategorie (z. B. eigenes Zimmer, Wohnzimmer, Playroom) |
| FR-07 | Zeitfenster schalten auf ein anderes Rechteprofil um (z. B. Nacht-Profil), sie sperren nicht. Nachts dimmen bzw. aus per Automatik. Admin und Eltern können jederzeit übersteuern |
| FR-08 | Automatik gemäß Kalender. Kalenderquelle ist der interne HA-Kalender (ADR-002) |
| FR-09 | Gäste nutzen nur die Tablets, Zugang per Gäste-Code, neu pro Besuch, gültig von–bis, gesteuert über den Kalender mit Freigabe durch Eltern |
| FR-10 | HomeKit als Alternative nur für Licht, Beschattung, Klima (Umfang tbd) |
| FR-11 | Fernzugriff ausschließlich über WireGuard-VPN |
| FR-12 | Backup der HA-Konfiguration auf das NAS, Restore getestet |
| FR-13 | Später: Ladestrom-Balancing (Hausbatterie, Auto-Batterie). Jetzt nur Erweiterbarkeit sichern |
| FR-14 | Bidirektionale Schnittstelle zu Loxone Gen1 als Eigenbau nach vorhandener API-Beschreibung (ADR-004) |
| FR-15 | Alarm-PIN pro Person, keine gemeinsame PIN. Prüfung nach Möglichkeit an der Quelle (SPC), siehe `04-rbac.md` |

## 5. Rechte-Grobmatrix (vorläufig, Details in Paket 2)

| Funktion | Admin | Eltern | Kind | Gast |
|---|---|---|---|---|
| Licht, Beschattung, Klima | ja | ja | zugeordnete Räume, Umfang je Rechteprofil (Tag, Nacht) | nur Gästebereich im Besuchszeitraum |
| Alarm-Status und Rückmeldung sehen | ja | ja | nur wenn für das Kind freigegeben (aus bis zur Freigabe, OP-08) | nein |
| Alarm scharf/unscharf (eigene PIN) | ja | ja | nur wenn für das Kind freigegeben (aus bis zur Freigabe, OP-08) | nein |
| Energie-Anzeige | ja | ja | ja | nein |
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
| OP-01 | Präzisiert (2026-11-25): Beide Kinder sind gleichberechtigt, es gibt keine Altersstufen. Alarm-Freigabe für Kinder siehe OP-08 | – |
| OP-02 | Erledigt (2026-11-25): Gäste-Code mit Gültigkeit von–bis, gesteuert über den Kalender | – |
| OP-03 | Erledigt (2026-11-25): Interner HA-Kalender (ADR-002). Funktionsumfang wird in Paket 2 geprüft | – |
| OP-04 | Erledigt (2026-11-26): Loxone-Schnittstelle wird gebaut (ADR-004), Prüfvorbehalt aufgehoben. Restpunkte siehe OP-11 | – |
| OP-05 | HomeKit-Umfang und Gästezugang | tbd |
| OP-06 | Erledigt (2026-11-25): HA bleibt als Container in Portainer, weil weitere Anwendungen auf dem Pi laufen (ADR-003). Der bestehende Container bleibt bis zur Freigabe unverändert | – |
| OP-07 | Erledigt (2026-11-25): Gäste-Code neu pro Besuch, Mechanismus nach ADR-006 (Code-Sperre auf dem Gäste-Dashboard, Freigabe durch Eltern). Machbarkeit ist vor der Umsetzung zu prüfen | – |
| OP-08 | Erledigt (2026-11-25): Alarm für Kinder aus, bis Admin freigibt. Energie-Anzeige für Kinder: ja | – |
| OP-09 | Erledigt (2026-11-25): Vorschlag in `04-rbac.md` bestätigt, konkrete Werte legt der Betreiber bei der Umsetzung fest | – |
| OP-10 | Erledigt (2026-11-25): akzeptiert. Kinder- und Gästegrenzen bei Komfortfunktionen sind eine Komfortgrenze, Alarm wird hart abgesichert (ADR-005) | – |
| OP-11 | Teilweise erledigt (2026-11-27): Firmware 17.3.11.10, interne Verbindung laut App unverschlüsselt. Offen: private API-Beschreibung mit der öffentlichen abgleichen, Verschlüsselungsmöglichkeit auf Gen1 prüfen | Bis dahin nur Haus-LAN mit dediziertem Konto |
| OP-12 | Erledigt (2026-11-27): Eigene FlexC-Engine, Anzeige und Bedienung in HA, gespeichertes SPC-Dienstkonto (ADR-007) | – |
| OP-13 | In der FlexC-Spezifikation prüfen: Kann der Code der Person je Befehl mitgegeben werden (PIN an der Quelle)? Hat das Dienstkonto nur Status- und Scharfschalt-Rechte, und ist ein eigener Benutzer für die Engine möglich? | Annahme: PIN-Prüfung in der Engine mit Audit-Log, Panel sieht das Dienstkonto |
| OP-14 | Fronius: Solar API an Dataloggern und Wechselrichtern lokal erreichbar? Gerätezuordnung und Werte mit Solar.web abgleichen | Annahme: erreichbar, Prüfung bei der Umsetzung |

## 9. Risiken

| ID | Risiko | Gegenmaßnahme |
|---|---|---|
| R-01 | Bestätigt (Paket 2): Home Assistant kennt nativ im Wesentlichen nur Admin und Nicht-Admin. Rechte pro Kind, Raum und Zeit sind nicht eingebaut, ausgeblendete Dashboards sind keine Sicherheitsgrenze | Schutzstufen-Modell nach ADR-005 (akzeptiert), Alarm an der Quelle absichern |
| R-02 | Loxone Gen1 ist ein Altgerät, Schnittstellen und Support sind begrenzt | Prüfung in Paket 3, Fallback-Optionen |
| R-03 | Alarm-Schalten aus HA vergrößert die Angriffsfläche | PIN, Rollenbeschränkung, Audit-Log, VPN-only |
| R-04 | Ausfall des Pi (Speichermedium) | Backup auf NAS, Restore-Test, DR-Plan in Paket 5 |
| R-05 | Fehlerhafte Kalenderautomatik schaltet Licht oder Beschattung unerwünscht | Manuelle Übersteuerung, Tests vor Freigabe |
| R-06 | Kiosk-Tablets lassen sich umgehen oder gestohlen werden | Eigene Kiosk-Konten mit minimalen Rechten, Härtung in Paket 4 |
| R-07 | Der Funktionsumfang des HA-Kalenders reicht für Gäste-Code und Automatik nicht aus | Prüfung in Paket 2, bei Bedarf neuer ADR (Option NAS-Kalender oder Eigenentwicklung) |
| R-08 | Der Pi teilt sich Ressourcen und Netz mit anderen Containern, HA ist kein dedizierter Host | Netztrennung, Ressourcenlimits, getrennte Datenablage in Paket 4 (ADR-003) |
| R-09 | Eigenbau der Loxone-Schnittstelle: Pflegeaufwand, Fehlverhalten kann Gebäudefunktionen beeinflussen | Spezifikation und Tests in Paket 3, Prüfung vorhandener Lösungen, Abschalten ohne Folgen für Loxone (NFR-01) |
| R-10 | Der Kalender in HA lässt sich nicht gegen Bearbeitung durch Kinder schützen | Besuch wird erst nach Freigabe durch Eltern aktiv (ADR-006) |
| R-11 | Ein auf einem Tablet gespeicherter Zugang kann bei Diebstahl oder Missbrauch alle Komfortfunktionen steuern | Tablet-Härtung (Paket 4), Token-Erneuerung bei Verlust, kein Zugriff aus dem Internet |
| R-12 | Gegenstandslos seit 2026-11-27: Die HA-Core-Integration für SPC wird nicht genutzt (ADR-007). Forenbericht: schaltet ohne Code | – |
| R-13 | Eigenbau FlexC-Engine: Pflegeaufwand, eigene Angriffsfläche, Panel-Firmware muss FlexC unterstützen (ab 3.6) | Eingrenzung auf Status und Schalten, getrennter Betrieb, Abschalten ohne Folgen für SPC (NFR-01) |
| R-14 | Gespeicherte SPC-Zugangsdaten der Engine erlauben Schalten. Das Panel sieht nur das Dienstkonto, nicht die Person | Dienstkonto mit minimalen Rechten, Secret-Ablage (Paket 4), PIN-Prüfung und Audit-Log in der Engine, Konto-Wechsel ohne Neubau |

## 10. Abnahme von Paket 1

Review der Anforderungen, der Annahmen und der offenen Punkte durch den Auftraggeber. Erst danach starten Paket 2 und 3.
