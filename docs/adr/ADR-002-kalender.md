# ADR-002: Kalender, interner HA-Kalender

- Status: Akzeptiert (Entscheidung Auftraggeber, 2026-11-25)
- Entscheider: Auftraggeber
- Betrifft: FR-07, FR-08, FR-09, OP-03, R-07

## Kontext

Der Kalender steuert die Nachtautomatik (dimmen bzw. aus), Zeitfenster und die Gültigkeit des Gäste-Codes (von–bis). Es gibt noch keinen Kalender. Ursprünglich war eine Eigenentwicklung mit Layout und Funktion wie Google Kalender online gewünscht.

## Optionen

| Option | Vorteile | Nachteile |
|---|---|---|
| A: Interner HA-Kalender | Teil von HA, keine zusätzliche Komponente, geringster Wartungs- und Sicherheitsaufwand, Automatisierungen greifen direkt darauf zu | Funktionsumfang und Bedienkomfort geringer als bei Google Kalender, Bearbeitungsrechte hängen am HA-Rechtemodell (R-01) |
| B: Kalender auf dem NAS (CalDAV) | Mehr Kalenderfunktionen, bekannte Apps nutzbar | Zusätzliche Komponente, zusätzlicher Zugangsweg, mehr Pflege |
| C: Eigenentwicklung (Google-ähnlich) | Volle Kontrolle über Layout und Funktion | Hoher Aufwand, dauerhafte Eigenwartung, eigene Sicherheitsfläche |

## Entscheidung

Option A: Der interne HA-Kalender wird genutzt.

## Warum

Er erfüllt den Zweck (Zeitfenster, Automatik, Gäste-Gültigkeit) mit der kleinsten Angriffsfläche und dem geringsten Pflegeaufwand. Das entspricht den Prioritäten Security, Stabilität und Wartbarkeit vor Features. Die Eigenentwicklung wäre ein eigenes Projekt neben der eigentlichen Plattform.

## Konsequenzen

- Der Wunsch nach Google-ähnlicher Bedienung wird nur im Rahmen des HA-Kalenders erfüllt. Der tatsächliche Umfang wird in Paket 2 geprüft.
- Vorschlag, in Paket 2 zu entscheiden: getrennte Kalender für Hausautomatik und Gäste.
- Wer Kalender bearbeiten darf, wird im RBAC-Konzept (Paket 2) festgelegt.
- Datenexport und Backup des Kalenders werden in Paket 5 geklärt.
- Reicht der Umfang nicht aus, wird ein neuer ADR mit Option B oder C erstellt.

## Risiken

R-01 (feingranulare Rechte), R-07 (Funktionsumfang reicht nicht für Gäste-Code und Automatik).

## Rollback

Wechsel auf Option B oder C über einen neuen ADR. Kalendereinträge müssen dafür exportierbar sein (Prüfung in Paket 5).
