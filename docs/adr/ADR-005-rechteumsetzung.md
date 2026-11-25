# ADR-005: Umsetzung des Rechtemodells (Schutzstufen und Durchsetzung)

- Status: Entwurf (Empfehlung des Architekten, Entscheidung durch den Auftraggeber)
- Datum: 2026-11-25
- Betrifft: FR-05, FR-06, FR-07, FR-09, R-01, R-11, OP-08 bis OP-10

## Kontext

Rechte sollen pro Kind, Raum, Kategorie und Zeit gelten, die Alarmanlage muss besonders geschützt sein. Home Assistant unterscheidet nativ im Wesentlichen Admin und Nicht-Admin. Nicht-Admins können weiterhin alle freigegebenen Entitäten sehen und bedienen, das Ausblenden in der Oberfläche ist keine Sicherheitsgrenze (Details und Quellen in `docs/04-rbac.md`).

## Optionen

| Option | Vorteile | Nachteile |
|---|---|---|
| A: Nur HA-Bordmittel (Nicht-Admin-Konten, Dashboards pro Nutzer, Kiosk-Ansicht) | Kein Zusatzaufwand, wartungsarm | Nur Oberflächen-Ebene. Wer die Schnittstelle direkt anspricht, umgeht alles. Für die Alarmanlage nicht akzeptabel |
| B: Schichtenmodell mit Schutzstufen: S1 Alarm an der Quelle (SPC) durchsetzen, S2 Komfort mit HA-Bordmitteln und Härtung, S3 nur Anzeige | Härteste Absicherung genau dort, wo es zählt. Wenig Eigenentwicklung. Loxone und SPC bleiben autark | Komfortgrenzen bei Kindern und Gästen sind nur Komfortgrenzen. Machbarkeit der SPC-seitigen PIN ist noch zu prüfen |
| C: Community-Komponente für Rechte (Zusatzmodul, das Aktionen abfängt) | Feinere Rechte ohne Eigenbau | Drittkomponente im sicherheitsrelevanten Pfad, Pflegezustand und Kompatibilität bei HA-Updates zu prüfen |
| D: Eigener Zugriffs-Vermittler vor HA, über den alle Clients laufen | Echte Durchsetzung auch für Komfortfunktionen | Hoher Entwicklungs- und Pflegeaufwand, eigene Sicherheitsfläche |

## Empfehlung

Option B. Als Ausbaupfad bleibt Option D, falls die Komfortgrenzen später nicht mehr reichen.

## Warum

Security hat Vorrang. Der Schaden bei Missbrauch ist bei der Alarmanlage hoch, bei Licht, Beschattung und Klima gering. Es ist deshalb sinnvoll, den Aufwand für harte Durchsetzung auf S1 zu konzentrieren und für S2 ein dokumentiertes Restrisiko zu akzeptieren. Das erhält Wartbarkeit und Stabilität und vermeidet eine Eigenentwicklung mit dauerhaftem Pflegeaufwand.

## Konsequenzen

- Jede Person mit Schaltrecht bekommt eine eigene Alarm-PIN. Die Prüfung soll an der Quelle erfolgen, Machbarkeit in Paket 3.
- Für Kinder und Gäste gibt es eigene Nicht-Admin-Konten, eigene Dashboards und gehärtete Tablets (Paket 4).
- Kinderrechte und Nacht-Profile sind Komfortgrenzen. Der Betreiber muss das akzeptieren (OP-10).
- Wenn die SPC keine personenbezogenen Codes mit eigenen Rechten bietet, ist ein neuer ADR nötig.

## Risiken

R-01, R-11. Restrisiko S2: Wer ein Zugangstoken eines Kinder- oder Gäste-Kontos erlangt, kann alle Komfortfunktionen steuern. Gegenmaßnahmen: Tablet-Härtung, kein Zugriff aus dem Internet, Token-Erneuerung bei Verlust.

## Rollback

Reine Konzeptentscheidung. Eine Änderung erfolgt über einen neuen ADR, der diesen ersetzt.
