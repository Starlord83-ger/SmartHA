# ADR-008: Fronius über die vorhandene HA-Integration (nur Anzeige)

Stand: 2026-11-26 · Status: Akzeptiert · Paket 3

- Status: Akzeptiert (Vorgabe Auftraggeber, 2026-11-26)
- Datum: 2026-11-26
- Entscheider: Auftraggeber
- Betrifft: FR-01, FR-04, FR-13, NFR-02, OP-14

## Kontext

Die Energieanlage (Primo 4.6-1, Symo Hybrid 5.0-3-S, Batteriemanagement, PowerMeter, zwei Datenlogger) soll in HA angezeigt werden. Später kommt Ladestrom-Balancing mit einem Elektroauto (FR-13).

## Optionen

| Option | Vorteile | Nachteile |
|---|---|---|
| A: HA-Core-Integration (lokale JSON-Schnittstelle) | Vorhanden, gepflegt, keine Cloud, kein Eigenbau | Funktionsumfang vorgegeben, Zuordnung bei zwei Wechselrichtern zu prüfen |
| B: Modbus-Community-Integration | Mehr Datenpunkte und Steuerung möglich | Für Gen24 gedacht, Drittanbieter, Schreibzugriff schon jetzt nicht nötig |
| C: Eigenbau | Volle Kontrolle | Aufwand ohne Nutzen für reine Anzeige |

## Entscheidung

Option A.

## Warum

Vorgabe des Auftraggebers. Für reine Anzeige ist die vorhandene Integration die kleinste und sicherste Lösung (Security und Wartbarkeit vor Features). Es entstehen keine Schreibrechte auf Energiegeräten.

## Konsequenzen

- Prüfung der lokalen Erreichbarkeit und Gerätezuordnung bei der Umsetzung (OP-14).
- Für Ladestrom-Balancing wird später ein eigener ADR angelegt, dann sind Schreibzugriffe und eine Neubewertung der Optionen nötig.

## Risiken

Unvollständige Daten bei zwei Wechselrichtern (zu prüfen, OP-14). Keine neuen Sicherheitsrisiken durch rein lesenden Zugriff im Haus-LAN.

## Rollback

Integration in HA entfernen. Fronius läuft unverändert weiter.
