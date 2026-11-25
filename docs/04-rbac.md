# 04 RBAC-Konzept

Stand: 2026-11-25 · Status: Freigegeben (2026-11-25) · Paket 2

## 1. Ziel

Ein eindeutiges Rechtemodell für Admin, Eltern, Kinder und Gäste. Es legt fest, **wer was wann** darf, und wo die Rechte **tatsächlich durchgesetzt** werden. Die technische Umsetzung folgt erst nach Freigabe (ADR-005, ADR-006).

## 2. Begriffe

| Begriff | Bedeutung |
|---|---|
| Rolle | Admin, Eltern, Kind, Gast |
| Person | Konkreter Mensch, jede Person hat eigene Zugangsdaten und eine eigene Alarm-PIN |
| Kategorie | Licht, Beschattung, Klima, Alarm, Energie |
| Raum | Zuordnung der Geräte, z. B. eigenes Zimmer, Wohnzimmer, Playroom, Gästebereich |
| Profil | Rechtesatz für ein Zeitfenster, z. B. Tag-Profil und Nacht-Profil |
| Schutzstufe | Wie hart ein Recht durchgesetzt werden muss (siehe Abschnitt 4) |

## 3. Rollen

| Rolle | Beschreibung |
|---|---|
| Admin | Verwaltet System, Rechte, Kalender und Konfiguration |
| Eltern | Bedienen alle Räume, sehen und schalten die Alarmanlage, pflegen den Kalender |
| Kind | Zwei Kinder, **gleichberechtigt**, keine Altersstufen. Rechte sind pro Kind, Raum und Kategorie konfigurierbar. Standardmäßig gelten für beide dieselben Werte |
| Gast | Nur Tablets, nur Gästebereich, nur im Zeitraum des Besuchs |

## 4. Schutzstufen

Home Assistant trennt nativ nur Admin und Nicht-Admin (siehe Abschnitt 9). Deshalb gilt ein gestuftes Modell:

| Stufe | Inhalt | Durchsetzung (Ziel) |
|---|---|---|
| S1 sicherheitskritisch | Alarmanlage: Status sehen, scharf/unscharf schalten | An der Quelle (SPC) mit PIN pro Person, nicht nur in HA |
| S2 Komfort mit Begrenzung | Licht, Beschattung, Klima für Kinder und Gäste | Konten, Dashboards und Geräte-Härtung in HA. Das ist eine **Komfortgrenze**, keine harte Sicherheitsgrenze |
| S3 Anzeige | Energie (Fronius) | Nur lesend |

Die Entscheidung dazu steht in ADR-005 (akzeptiert).

## 5. Rechtematrix

| Funktion | Admin | Eltern | Kind | Gast |
|---|---|---|---|---|
| Licht, Beschattung, Klima | alle Räume | alle Räume | nur zugeordnete Räume, Umfang je Profil | nur Gästebereich, nur im Besuchszeitraum |
| Alarm-Status und Rückmeldung | ja | ja | nur wenn für das Kind freigegeben (aus bis zur Freigabe durch Admin, OP-08 entschieden) | nein |
| Alarm scharf/unscharf, mit eigener PIN | ja | ja | nur wenn für das Kind freigegeben (aus bis zur Freigabe durch Admin, OP-08 entschieden) | nein |
| Energie-Anzeige | ja | ja | ja (OP-08, entschieden) | nein |
| Kalender ansehen und bearbeiten | ja | ja | nein (Annahme) | nein |
| Rechte, Räume, Profile, Konten verwalten | ja | nein | nein | nein |
| Gästebesuch anlegen und freigeben | ja | ja | nein | nein |

## 6. Zeitfenster als Profile

Ein Zeitfenster **sperrt nicht**, sondern schaltet auf ein anderes Rechteprofil um.

| Profil | Gilt | Inhalt (Vorschlag, bestätigt, OP-09) |
|---|---|---|
| Tag | Standard | Rechte laut Zuordnung pro Kind, Raum, Kategorie |
| Nacht | Zeitfenster laut Kalender | Engere Grenzen, z. B. Licht nur bis zu einer gedimmten Stufe, Beschattung und Klima nur im eigenen Zimmer oder nicht änderbar |

- Das aktive Profil wird über den Kalender gesteuert (ADR-002).
- Admin und Eltern können jederzeit manuell übersteuern.
- Die konkreten Grenzwerte pro Profil legt der Betreiber fest (OP-09).

## 7. Alarm-PIN pro Person

- Jede Person, die schalten darf, hat eine eigene PIN. Es gibt keinen gemeinsamen Familien-PIN.
- Ziel: Die PIN wird an der Quelle (SPC) geprüft, weil nur dort die Durchsetzung auch dann greift, wenn jemand HA umgeht. Ob die SPC personenbezogene Codes mit eigenen Rechten bietet und wie sie sich anbinden lassen, wird in Paket 3 geprüft.
- Fallback, falls das nicht geht: Prüfung in HA. Das ist schwächer und wird dann in einem ADR als Restrisiko festgehalten.
- Nach jeder Aktion erscheint eine Rückmeldung. Wer wann geschaltet hat, soll nachvollziehbar sein (SPC-Protokoll, HA-Logbuch).
- PINs stehen nie im Repository oder in der Dokumentation.

## 8. Gäste-Code

Regel: Der Code wird **neu pro Besuch** erzeugt und gilt nur im Zeitraum von–bis.

Soll-Ablauf:

1. Admin oder Eltern legen den Besuch im Kalender an (von–bis).
2. Eltern geben den Besuch frei. Ein Kalendereintrag allein schaltet nichts frei, weil der Kalender in HA nicht gegen Bearbeitung durch Kinder geschützt werden kann (R-10).
3. Zu Beginn wird ein neuer Code aktiv, am Ende wird er ungültig.
4. Der Code gibt nur Zugriff auf den Gästebereich der Gäste-Tablets.

Wie der Code technisch erzeugt und geprüft wird, steht in ADR-006 (akzeptiert).

## 9. Befund: Was Home Assistant nativ kann

Stand der Prüfung: 2026-11-25. Quellen im Abschnitt 11.

- Es gibt die Systemgruppen Admin, Benutzer und Nur-Lesen. Alle haben Zugriff auf alle Entitäten, Nur-Lesen kann nichts steuern. Die Entwickler haben das Rechtesystem selbst als unfertig bezeichnet.
- Nicht-Admins können weiterhin alle freigegebenen Entitäten und Dashboards sehen und bedienen. Das Ausblenden von Dashboards oder Ansichten wirkt nur in der Oberfläche und ist **keine Sicherheitsgrenze**.
- Feingranulare Rechte pro Kind, Raum oder Zeit sind ein offener Wunsch der Community. Es gibt Zusatzkomponenten von Dritten, deren Eignung und Pflege zu prüfen wäre.
- Der lokale Kalender ist für Automatisierungen gedacht, nicht als vollwertiger Personenkalender. Automatisierungen können auf Beginn und Ende von Terminen reagieren. Einen Auslöser für „neuer Termin angelegt“ gibt es laut Community-Berichten nicht.

Folgerung: Eine echte Durchsetzung pro Kind, Raum und Zeit ist mit Bordmitteln nicht möglich. Deshalb das Stufenmodell.

## 10. Offene Punkte und Risiken

| ID | Inhalt |
|---|---|
| OP-07 | Erledigt (2026-11-25): Mechanismus nach ADR-006 (akzeptiert). Machbarkeit vor der Umsetzung zu prüfen |
| OP-08 | Erledigt (2026-11-25): Alarm für Kinder aus, bis Admin freigibt. Energie-Anzeige für Kinder: ja |
| OP-09 | Erledigt (2026-11-25): Vorschlag bestätigt. Konkrete Grenzwerte legt der Betreiber bei der Umsetzung fest |
| OP-10 | Erledigt (2026-11-25): akzeptiert. Kinder- und Gästegrenzen bei Komfortfunktionen sind eine Komfortgrenze, keine harte Sicherheitsgrenze |
| R-01 | Keine native Rechtedurchsetzung in HA (siehe Abschnitt 9) |
| R-10 | Kalender in HA nicht gegen Bearbeitung durch Kinder schützbar |
| R-11 | Ein auf einem Tablet gespeicherter Zugang kann bei Diebstahl oder Missbrauch alle Komfortfunktionen steuern |

## 11. Quellen

- Entwicklerblog zu Benutzerrechten: https://developers.home-assistant.io/blog/2019/03/11/user-permissions.html
- Diskussion zu Zugriffsrechten pro Benutzer: https://github.com/orgs/home-assistant/discussions/1171
- Community-Thread zu fehlendem RBAC: https://community.home-assistant.io/t/wth2-wth-no-rbac-role-based-access-control-users-groups-rights/467158
- Lokaler Kalender: https://www.home-assistant.io/integrations/local_calendar/
- Kalender allgemein: https://home-assistant.io/components/calendar/
- Community zu fehlendem Auslöser „neuer Termin“: https://community.home-assistant.io/t/local-calendar-automation-trigger-new-event-added/501533
- Beispiel einer Drittkomponente: https://github.com/SamAthanas/user-rbac

## 12. Abnahme

Jede Zeile der Rechtematrix ist eindeutig, OP-08 bis OP-10 sind beantwortet, ADR-005 und ADR-006 sind akzeptiert.
