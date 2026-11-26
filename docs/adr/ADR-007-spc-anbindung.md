# ADR-007: SPC-Anbindung (Weg zur Alarmanlage)

Stand: 2026-11-26 · Status: Entwurf · Paket 3

- Status: Entwurf (Entscheidung des Auftraggebers offen)
- Datum: 2026-11-26
- Entscheider: Auftraggeber
- Betrifft: FR-03, FR-15, NFR-01, NFR-02, OP-12, OP-13, R-03, R-12, R-13

## Kontext

HA soll den Alarmstatus anzeigen und scharf/unscharf schalten. Die PIN gilt pro Person und soll nach ADR-005 möglichst an der Quelle (SPC) geprüft werden (Schutzstufe S1). Der Auftraggeber hat die FlexC-Spezifikation und Zugang zur SPC-HA-Integration und nennt zwei Wege: selbst bauen oder die HA-Integration nutzen.

## Optionen

| Option | Vorteile | Nachteile |
|---|---|---|
| A: HA-Core-Integration über das Drittanbieter-Gateway | In HA vorhanden, wenig Eigenaufwand, Status und Schalten sofort nutzbar | Zusätzliche Komponente (Gateway), Integration „legacy“, laut Forenbericht Schalten ohne Code, PIN-Prüfung nur in HA |
| B: Community-Integration `spc_web` | Kein Gateway nötig | Lockert TLS-Sicherheit bewusst, Drittanbieter ohne Zusicherung, nur lokal oder per VPN |
| C: Eigenbau über FlexC | Volle Kontrolle, PIN-Prüfung an der Quelle möglich, falls das Protokoll es hergibt, Spezifikation vorhanden | Höchster Aufwand und Pflege, eigene Sicherheitsfläche (R-13), Machbarkeit der PIN pro Person offen (OP-13) |
| D: A für Anzeige, C später für Schalten | Schnell sichtbarer Nutzen, harte Absicherung folgt nach Prüfung | Zwei Etappen, Übergangslösung für das Schalten nötig |

## Empfehlung (Entscheidung steht aus)

Option D in dieser Reihenfolge:

1. **Status anzeigen** über Option A. Lesen ist unkritisch und liefert schnell Nutzen.
2. **Schalten** erst nach Prüfung der FlexC-Spezifikation (OP-13). Ist eine PIN pro Person an der Quelle möglich, wird Option C für das Schalten gebaut. Ist sie es nicht, bleibt das Schalten in HA mit PIN-Prüfung und Rollenbeschränkung, und das Restrisiko wird in einem eigenen ADR festgehalten (so schon in `04-rbac.md` Abschnitt 7 vorgesehen).

## Warum

- Security hat Vorrang: Das Schalten ist der kritische Teil (R-03), das Anzeigen nicht. Es lohnt sich, die harte Variante erst nach der Spezifikationsprüfung zu entscheiden.
- Stabilität: Option A nutzt Vorhandenes und verkleinert den Eigenbau auf das, was wirklich nötig ist.
- Wartbarkeit: Jeder Eigenbau (Loxone ist bereits einer, ADR-004) bindet dauerhaft Pflegeaufwand des Auftraggebers.

## Konsequenzen

- OP-12: Entscheidung des Auftraggebers über den Weg.
- OP-13: FlexC-Spezifikation und Panel prüfen: personenbezogene Codes, Rechte, Protokollierung, Schalten durch Fremdsystem.
- Die Spezifikation kommt nicht ins Repository, wenn sie lizenzgebunden ist. Ablageort und Verweis klärt der Auftraggeber (Paket 4).
- Umsetzung nur nach ausdrücklicher Freigabe.

## Risiken

R-03 (Angriffsfläche durch Schalten aus HA), R-12 (Schalten ohne Code), R-13 (Eigenbau FlexC).

## Rollback

Anbindung abschalten, SPC läuft unverändert weiter (NFR-01). Wechsel zwischen den Optionen ist möglich, da HA nur eine Alarmentität nach außen zeigt.
