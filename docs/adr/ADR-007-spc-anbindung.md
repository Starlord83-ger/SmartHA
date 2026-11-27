# ADR-007: SPC-Anbindung über eine eigene FlexC-Engine

Stand: 2026-11-27 · Status: Akzeptiert · Paket 3

- Status: Akzeptiert (Entscheidung Auftraggeber, 2026-11-27)
- Datum: 2026-11-27
- Entscheider: Auftraggeber
- Betrifft: FR-03, FR-15, NFR-01, NFR-02, NFR-04, OP-12, OP-13, R-03, R-12, R-13, R-14

## Kontext

HA soll den Alarmstatus anzeigen und scharf/unscharf schalten. Die PIN gilt pro Person (FR-15), Schutzstufe S1 (ADR-005). Der Auftraggeber hat die FlexC-Spezifikation und Zugang zum Panel. Am 2026-11-26 lagen die Optionen A bis D vor, empfohlen war D (Anzeige über die HA-Integration, Schalten nach Prüfung).

## Optionen

| Option | Vorteile | Nachteile |
|---|---|---|
| A: HA-Core-Integration über Drittanbieter-Gateway | Vorhanden, wenig Eigenaufwand | Zusätzliche Komponente, „legacy“, laut Forenbericht Schalten ohne Code |
| B: Community-Integration `spc_web` | Kein Gateway | Lockert TLS bewusst, Drittanbieter |
| C: Eigene FlexC-Engine (Status und Schalten) | Volle Kontrolle, ein Weg für beides, Spezifikation vorhanden, keine Fremdkomponente | Höchster Aufwand und Pflege, eigene Sicherheitsfläche |
| D: A für Anzeige, C später für Schalten | Schneller Nutzen | Zwei Etappen, Übergang nötig |

## Entscheidung

Option C. Eine eigene FlexC-Engine liefert Status und nimmt Schaltbefehle an, HA zeigt an und bedient sie. Die Engine meldet sich mit einem gespeicherten SPC-Benutzernamen und -Passwort am Panel an. Die Zugangsdaten liegen in der Secret-Ablage (Paket 4), nie im Repository.

## Warum

Entscheidung des Auftraggebers: ein durchgängiger Weg ohne Drittkomponente. Das vermeidet das Gateway und die Abhängigkeit von einer „legacy“-Integration und hält die Schnittstelle an einer Stelle. Das passt zu ADR-004 (ebenfalls Eigenbau).

Abweichung von meiner Empfehlung (Option D): Ich hatte vorgeschlagen, erst die FlexC-Spezifikation zu prüfen. Das bleibt als Prüfpunkt (OP-13), ändert aber den Weg nicht mehr. Nachteil: dauerhafter Pflegeaufwand beim Auftraggeber (R-13).

## Konsequenzen

- **Ein Konto für die Engine:** Das Panel sieht nur das gespeicherte Dienstkonto, nicht die einzelne Person. Eine PIN pro Person (FR-15) wird deshalb von der Engine geprüft, nicht vom Panel, es sei denn FlexC erlaubt, den Code der Person je Befehl mitzugeben (OP-13). Das Panel-Protokoll zeigt dann das Dienstkonto, die Zuordnung zur Person steht im Audit-Log der Engine.
- **Prüfung in der Engine, nicht in der HA-Oberfläche:** Sonst könnte jeder mit HA-Zugang die PIN umgehen. Die Engine nimmt Schaltbefehle nur von HA an und nur mit gültiger PIN der Person.
- **Schutz des Dienstkontos:** Das gespeicherte Konto kann schalten. Es bekommt nur die nötigen SPC-Rechte (Status und Scharfschalten, keine Programmierung), liegt in der Secret-Ablage und lässt sich ohne Neubau tauschen (R-14).
- **Annahme zu klären (OP-13):** Das Dienstkonto darf laut Panel-Rechten scharf/unscharf schalten und ein eigener Benutzer für die Engine ist möglich.
- Die Spezifikation kommt nicht ins Repo, wenn lizenzgebunden. Ablageort in Paket 4.
- Die Engine läuft getrennt von HA (eigener Container), damit ein HA-Ausfall SPC nicht beeinflusst (NFR-01). Ort und Netzzone in Paket 4.
- Umsetzung nur nach ausdrücklicher Freigabe.

## Risiken

R-03 (Angriffsfläche durch Schalten), R-12 (entfällt für den gewählten Weg, da die HA-Integration nicht genutzt wird), R-13 (Pflege und Sicherheit des Eigenbaus), R-14 (gespeicherte Zugangsdaten mit Schaltrechten).

## Rollback

Engine abschalten, SPC läuft unverändert weiter (NFR-01). Rückfall auf Option A oder B ist möglich, weil HA nur eine Alarmentität nach außen zeigt.
