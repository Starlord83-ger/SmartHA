# SECURITY.md

Privates Repository eines Haushalts. Diese Datei beschreibt, wie mit Geheimnissen und sicherheitsrelevanten Änderungen umgegangen wird.

## Was nie ins Repository oder in Chats gehört

Passwörter, API-Tokens, PINs, Gäste-Codes, WireGuard-Schlüssel, Zertifikatsschlüssel, SPC- und Loxone-Zugangsdaten, `secrets.yaml`, Backups mit Zugangsdaten. In der Dokumentation steht nur, **wo** ein Geheimnis liegt, nie sein Wert. Der Ablageort wird vom Betreiber festgelegt (Paket 4).

## Grundsätze

- Least Privilege: HA nutzt dedizierte Konten mit minimalen Rechten auf Loxone, SPC und Fronius.
- Kein Zugriff aus dem Internet. Fernzugriff nur per WireGuard-VPN.
- Die Alarmanlage wird nie an HomeKit exponiert. Schalten nur mit PIN und nur für berechtigte Rollen.
- Loxone und SPC bleiben bei einem HA-Ausfall voll funktionsfähig.

## Sicherheitsrelevante Änderungen

Änderungen an Alarm, VPN, Rechten, Netzwerkzonen oder Zugangsdaten brauchen einen ADR, eine CHG-Beschreibung mit Rollback und die ausdrückliche Freigabe des Betreibers.

## Wenn ein Geheimnis versehentlich im Repository landet

1. Geheimnis sofort ändern bzw. widerrufen.
2. Danach Historie bereinigen und prüfen, wo es noch liegt.
3. Vorfall im `docs/PROJECT_LOG.md` ohne den Wert festhalten.

## Prüfungen

Secret-Scan und weitere Automatisierung werden in Paket 4 festgelegt. Bis dahin gelten `.gitignore`, die Sperren in `.claude/settings.json` und der Review im Pull Request.

## Meldeweg

Der Betreiber des Repositories ist die einzige Ansprechperson.
