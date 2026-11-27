# ADR-004: Loxone Gen1, bidirektionale Schnittstelle als Eigenbau

- Status: Akzeptiert (Vorgabe Auftraggeber 2026-11-25, Prüfvorbehalt am 2026-11-26 aufgehoben)
- Entscheider: Auftraggeber
- Betrifft: FR-02, FR-14, NFR-01, NFR-02, OP-04, R-09

## Kontext

Home Assistant soll Zustände aus Loxone lesen und Licht, Beschattung und Klima schalten (bidirektional). Der Miniserver ist Gen1. Eine API-Beschreibung liegt dem Auftraggeber vor.

## Optionen

| Option | Vorteile | Nachteile |
|---|---|---|
| A: Eigenbau nach der vorhandenen API-Beschreibung | Volle Kontrolle über Funktionen und Rechte, passt exakt zu Gen1 und zu den Rollen | Entwicklungs- und Pflegeaufwand, eigene Fehler- und Sicherheitsfläche |
| B: Vorhandene Community-Lösung nutzen | Weniger Eigenaufwand | Abdeckung von Gen1 und Pflegezustand zu prüfen, Abhängigkeit von Dritten |
| C: Zwischenschicht (z. B. Nachrichtenvermittler) | Entkopplung | Zusätzliche Komponente |

## Entscheidung

Option A: Die bidirektionale Schnittstelle wird gebaut, auf Basis der vorhandenen API-Beschreibung.

## Warum

Vorgabe des Auftraggebers. Ein Eigenbau erlaubt ein dediziertes Loxone-Konto mit minimalen Rechten und eine Abbildung genau der benötigten Funktionen (Least Privilege). Als Architekt empfehle ich trotzdem, vor dem Start zu prüfen, ob B den Eigenbau ersetzen oder verkleinern kann, weil der Wartungsaufwand dauerhaft bei dir liegt.

## Prüfvorbehalt (aufgehoben 2026-11-26)

In Paket 3 sollte geprüft werden, ob B oder C Teile abdecken. Der Auftraggeber hat am 2026-11-26 bestätigt, dass die Schnittstelle gebaut wird und die Spezifikation vorliegt. Der Eigenbau bleibt damit bei Option A. Offen bleiben Firmware-Version und Abgleich der privaten API-Beschreibung (OP-11). Spezifikation: `docs/05-interfaces.md`.

## Konsequenzen

- Paket 3 liefert eine Schnittstellenspezifikation (Datenmodell, Fehlerverhalten, Sicherheit, Tests) auf Basis der API-Beschreibung.
- Implementierung beginnt nur nach ausdrücklicher Freigabe.
- Die API-Beschreibung und die Firmware-Version des Miniservers werden als Eingang für Paket 3 benötigt (ohne Zugangsdaten).
- Loxone bleibt bei Ausfall der Schnittstelle oder von HA unverändert funktionsfähig (NFR-01).

## Risiken

R-09 (Aufwand, Wartung und Sicherheit des Eigenbaus). Fehlverhalten kann Gebäudefunktionen beeinflussen.

## Rollback

Schnittstelle abschalten. Loxone läuft eigenständig weiter und ist über seine eigenen Bedienwege nutzbar.
