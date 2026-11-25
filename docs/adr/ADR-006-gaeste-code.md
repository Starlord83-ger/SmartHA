# ADR-006: Gäste-Code, neu pro Besuch, kalendergesteuert

- Status: Akzeptiert (Empfehlung des Architekten, vom Auftraggeber am 2026-11-25 bestätigt)
- Datum: 2026-11-25
- Betrifft: FR-09, OP-07, R-10, R-11

## Kontext

Gäste nutzen nur die Tablets. Der Code wird **neu pro Besuch** erzeugt und gilt nur von–bis. Home Assistant sieht keine Code-Anmeldung für Gäste vor (zu prüfen). Der lokale Kalender kann Automatisierungen bei Beginn und Ende von Terminen auslösen. Einen Auslöser für „neuer Termin“ gibt es laut Community-Berichten nicht. Der Kalender lässt sich nicht gegen Bearbeitung durch Kinder schützen (R-10).

## Optionen

| Option | Vorteile | Nachteile |
|---|---|---|
| A: Code-Sperre auf dem Gäste-Dashboard. Zu Beginn des Besuchs wird ein neuer Code gesetzt, am Ende ungültig | Lösbar mit HA-Bordmitteln, Code pro Besuch, passt zu Kalender | Prüfung nur auf Oberflächen-Ebene. Der Code muss in HA gespeichert werden |
| B: Gästekonto wird zu Beginn aktiviert und am Ende deaktiviert, der Code ist das Passwort des Gästekontos | Echte Anmeldung | Automatisches Setzen von Passwörtern und Aktivieren von Konten ist in HA nicht vorgesehen (Machbarkeit zu prüfen) |
| C: Eigener Zugriffs-Vermittler (siehe ADR-005, Option D) | Echte Prüfung, volle Kontrolle | Hoher Entwicklungs- und Pflegeaufwand |

## Entscheidung

Option A (vom Auftraggeber am 2026-11-25 bestätigt), ergänzt um einen Freigabeschritt durch Eltern: Ein Kalendereintrag allein schaltet nichts frei, erst die Bestätigung durch Admin oder Eltern aktiviert den Besuch.

## Warum

Option A ist mit den vorhandenen Mitteln machbar, hält den Aufwand klein und erfüllt „neu pro Besuch“. Weil der Kalender nicht gegen Änderungen durch Kinder zu schützen ist, braucht es den Freigabeschritt. Gäste haben ohnehin keine Alarm- und Energie-Rechte, daher ist das Restrisiko auf Komfortfunktionen im Gästebereich begrenzt (S2).

## Konsequenzen

- Der Code steht nie in Repository oder Dokumentation. Ablageort und Erneuerung werden in Paket 4 festgelegt.
- Nach Ende des Besuchs ist der Code automatisch ungültig, ein neuer Besuch erzeugt einen neuen Code.
- Gäste bekommen keinen Zugriff auf Kalender, Alarm oder Energie.
- Falls A technisch nicht trägt, wird B geprüft oder ein neuer ADR angelegt.

## Risiken

R-10 (Kalender-Bearbeitung), R-11 (Token auf dem Tablet). Machbarkeit der Code-Sperre und der automatischen Erneuerung ist vor der Umsetzung zu prüfen.

## Rollback

Reine Konzeptentscheidung. Änderung über einen neuen ADR.
