# Operatoranleitung

Stand: 2026-11-25 · Status: Gerüst (Soll-Beschreibung) · fortlaufend gepflegt

## Zweck und Status

Diese Anleitung beschreibt, wie Features konfiguriert und angewendet werden. **Noch ist kein Feature umgesetzt.** Die Abschnitte beschreiben deshalb den **Soll-Zustand** nach den bisherigen Entscheidungen. Konkrete Bedienschritte werden mit dem jeweiligen Release ergänzt. Ein Feature gilt erst als fertig, wenn sein Abschnitt auf „verfügbar“ steht.

Statuswerte je Feature: geplant, in Arbeit, verfügbar.

## Rollen im Überblick

| Rolle | Kurzbeschreibung |
|---|---|
| Admin | Verwaltet System, Rechte, Konfiguration |
| Eltern | Bedienen alles Fachliche, sehen und schalten die Alarmanlage mit PIN |
| Kinder | Nur freigegebene Räume und Kategorien, nur in Zeitfenstern. Ältere Kinder können mit PIN die Alarmanlage schalten und Status sehen |
| Gast | Nur Tablets, Gäste-Code mit Gültigkeit von–bis, nur Gästebereich |

## Features

### 1. Licht, Beschattung, Klima bedienen

- **Wer:** Admin, Eltern, Kinder (freigegebene Räume, Zeitfenster), Gäste (Gästebereich)
- **Ablauf (Soll):** Bedienung über Tablet oder HA-Oberfläche. Die Befehle laufen über die Loxone-Schnittstelle (ADR-004).
- **Status:** geplant. Konkrete Schritte folgen mit P3 und der Umsetzung.

### 2. Alarmanlage: Status sehen, scharf/unscharf schalten

- **Wer:** Status sehen: Admin, Eltern, ältere Kinder. Schalten: Admin, Eltern, ältere Kinder, jeweils mit PIN.
- **Regeln:** Gäste sehen nichts. Die Alarmanlage ist nicht über HomeKit erreichbar. Nach jeder Aktion erscheint eine Rückmeldung.
- **PIN:** Eine eigene PIN pro Person, kein gemeinsamer PIN. Wird nie im Repository oder in dieser Anleitung dokumentiert. Verwaltung und Ablage legt P4 fest.
- **Status:** geplant (P2, P3).

### 3. Energieanzeige (Fronius)

- **Wer:** Admin, Eltern (Annahme).
- **Ablauf (Soll):** Nur Anzeige. Steuerung ist nicht vorgesehen.
- **Später:** Ladestrom-Balancing zwischen Haus- und Auto-Batterie, sobald ein Elektroauto vorhanden ist. Eigener Planungsschritt.
- **Status:** geplant (P3).

### 4. Rechte der Kinder konfigurieren

- **Wer:** Admin (Eltern nach Festlegung in P2).
- **Konfigurierbar (Soll):** pro Kind, pro Raum, pro Kategorie (Licht, Beschattung, Klima, Alarm) und pro Zeitfenster.
- **Ablauf (Soll):** Rechteprofile je Zeitfenster (Tag, Nacht) statt Sperre, Details in `04-rbac.md`. Home Assistant bietet diese Rechte nicht nativ (R-01, ADR-005). Beide Kinder sind gleichberechtigt.
- **Status:** geplant (P2).

### 5. Nachtautomatik und Kalender

- **Wer:** Admin und Eltern pflegen den Kalender, Bearbeitungsrechte werden in P2 festgelegt.
- **Ablauf (Soll):** Licht wird nachts gedimmt bzw. ausgeschaltet und gemäß Kalender wieder eingeschaltet. Kalender ist der interne HA-Kalender (ADR-002).
- **Übersteuerung:** Eine manuelle Bedienung muss die Automatik übersteuern können (Risiko R-05).
- **Status:** geplant (P2).

### 6. Gäste-Code

- **Wer:** Admin und Eltern legen den Gastzeitraum an.
- **Ablauf (Soll):** Gastzeitraum als Kalendereintrag von–bis. Außerhalb des Zeitraums ist der Zugang gesperrt.
- **Regel:** Der Code wird neu pro Besuch erzeugt und gilt nur im Zeitraum von–bis. Ein Besuch wird erst nach Freigabe durch Eltern aktiv. Die technische Prüfung ist als Entwurf in ADR-006 beschrieben (OP-07).
- **Status:** geplant (P2).

### 7. Kiosk-Tablets

- **Ablauf (Soll):** Lenovo-Tablets laufen im Kiosk-Modus mit eigenen Konten und minimalen Rechten. Gäste nutzen nur diese Tablets.
- **Härtung:** Wird in P4 festgelegt.
- **Status:** geplant (P4).

### 8. Fernzugriff

- **Ablauf (Soll):** Ausschließlich über WireGuard-VPN. Es gibt keine Freigabe ins Internet.
- **Status:** geplant (P4).

### 9. HomeKit

- **Umfang:** Alternative nur für Licht, Beschattung, Klima. Gästezugang offen (OP-05).
- **Status:** geplant (tbd).

## Betrieb

### Änderungen und Updates

Nie ohne Freigabe. Vor einem Update wird gesichert, das Image mit festem Tag gesetzt und der Rollback bereitgehalten. Siehe `runbooks/update.md`.

### Backup und Restore

Verfahren und Test werden in P5 festgelegt. Siehe `runbooks/restore.md`.

### Wenn Home Assistant ausfällt

Loxone und SPC arbeiten unverändert weiter (NFR-01) und bleiben über ihre eigenen Bedienwege nutzbar. Danach Wiederherstellung nach Runbook.

## Pflege dieser Anleitung

Mit jedem Release ergänzt der Betreiber bzw. Claude die Abschnitte um konkrete Konfigurations- und Bedienschritte, Prüfschritte und Fehlerbehebung. Änderungen werden im `PROJECT_LOG.md` und im `CHANGELOG.md` vermerkt.
