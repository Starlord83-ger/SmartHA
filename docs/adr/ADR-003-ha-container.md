# ADR-003: Home Assistant bleibt als Container in Portainer

- Status: Akzeptiert (Entscheidung Auftraggeber, 2026-11-25)
- Entscheider: Auftraggeber
- Betrifft: OP-06, R-08, NFR-05, NFR-06

## Kontext

Home Assistant läuft bereits als Container in Portainer auf dem Raspberry Pi 5, ist aber noch nicht konfiguriert. Auf dem Pi laufen weitere Anwendungen. Das Projektziel sah ursprünglich einen dedizierten HA-Host vor.

## Optionen

| Option | Vorteile | Nachteile |
|---|---|---|
| A: Container in Portainer behalten | Läuft bereits, andere Anwendungen bleiben unberührt, Portainer-Verwaltung vorhanden | Keine Add-ons und kein Supervisor, Updates und Backups manuell, Pi ist gemeinsamer Ausfallpunkt |
| B: HA OS auf dediziertem Gerät | Add-ons, eingebaute Backups, am besten unterstützt | Pi wäre nur noch für HA da, andere Anwendungen müssten umziehen |
| C: Supervised-Installation auf dem Pi | Add-ons möglich | Erfordert in der Regel einen dedizierten Host (zu prüfen), hohe Komplexität |

## Entscheidung

Option A: HA bleibt als Container in Portainer. Der bestehende Container bleibt unverändert, bis eine Änderung freigegeben ist.

## Warum

Andere Anwendungen laufen auf dem Pi und sollen weiterlaufen. Der bestehende Container verursacht keine Umbaukosten und kein Risiko für diese Anwendungen. Option B wäre nur mit einem zusätzlichen Gerät sinnvoll.

## Konsequenzen

- Zusatzdienste, die sonst als Add-on liefen, werden bei Bedarf als eigene Container betrieben.
- Updates erfolgen kontrolliert über Portainer mit festem Image-Tag (kein `latest`), nur nach Freigabe.
- Das Backup-Verfahren (Sicherung der HA-Daten auf das NAS, Restore-Test) wird in Paket 5 festgelegt.
- Netzwerkmodus, Ressourcenlimits und die Trennung von anderen Containern werden in Paket 4 geklärt. Dabei ist zu prüfen, welcher Netzwerkmodus für HomeKit und die Geräteerkennung nötig ist.
- Die Annahme „dedizierter HA-Host“ entfällt. Sie ist in den Anforderungen angepasst.

## Risiken

R-04 (Pi-Ausfall), R-08 (gemeinsame Ressourcen und Wechselwirkungen mit anderen Containern).

## Rollback

Image-Tag auf den vorherigen Stand zurücksetzen und die Datensicherung wiederherstellen. Details im Runbook (`docs/runbooks/`).
