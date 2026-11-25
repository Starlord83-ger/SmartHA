# ADR-001: Frontend-Strategie

- Status: Akzeptiert (Vorgabe Auftraggeber, 2026-11-25)
- Entscheider: Auftraggeber
- Betrifft: Tablets, Mobilgeräte, HomeKit, RBAC

## Kontext

Familie und Gäste sollen Licht, Beschattung, Klima, Alarm-Status und Energie bedienen oder sehen. Es gibt vier Rollen, und die Rechte der Kinder müssen pro Kind, Raum und Kategorie sowie nach Zeitfenstern einstellbar sein. Alarm-Funktionen dürfen nicht für alle sichtbar sein.

## Optionen

| Option | Vorteile | Nachteile |
|---|---|---|
| A: HA-Dashboards primär, HomeKit alternativ | Ein Leitstand für alle Systeme, volle Anzeige inkl. Alarm und Energie, passt zu Kiosk-Tablets | Feingranulare Rechte nicht nativ (Risiko R-01), Pflege der Dashboards |
| B: HomeKit primär | Vertraute Bedienung für die Familie | Kein Alarm- und Energie-Leitstand, Rechtemodell grob, Zeitfenster und Kalenderlogik begrenzt |
| C: Loxone-App primär | Bereits vorhanden | Umgeht das Ziel eines zentralen Leitstands, keine Sicht auf SPC und Fronius |

## Entscheidung

Option A. Home Assistant ist das primäre Frontend. HomeKit ist ein alternatives Frontend und deckt nur Licht, Beschattung und Klima ab. Die Alarmanlage wird nicht an HomeKit exponiert.

## Konsequenzen

- Gäste nutzen ausschließlich die Tablets mit Gäste-Code (HomeKit-Gastzugang bleibt tbd).
- Die Umsetzung der Rechte pro Kind, Raum, Kategorie und Zeit wird in einem eigenen ADR (Paket 2) entschieden. Dashboard-Ausblenden gilt nicht als Sicherheitsmaßnahme.
- HomeKit-Umfang und Gästezugang werden vor der Umsetzung erneut bewertet.

## Risiken

- R-01 (feingranulare Rechte), R-06 (Tablet-Umgehung). Siehe Anforderungsanalyse.

## Rollback

Reine Dokumentationsentscheidung. Eine Änderung erfolgt über einen neuen ADR, der diesen ablöst.
