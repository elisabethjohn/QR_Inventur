# Inventur Scan

Web-App für die Inventur von Chemikalien mit dem Smartphone. Jedes Gebinde trägt einen QR-Code mit seiner Umgang-Id, jeder Raum einen Raum-QR-Code. Die App scannt beides, gleicht mit der Bestandsliste (Excel) ab und zeigt sofort, was im Raum noch fehlt.

Läuft komplett im Browser, funktioniert nach dem ersten Laden auch ohne Internet und lässt sich wie eine App auf dem Home-Bildschirm installieren. Es gibt keinen Server und kein Benutzerkonto.

**Aktuelle Version:** 1.2

## Funktionen

- **Live-Scan mit der Handykamera** (QR-Codes), mit Zoom-Regler und Tipp-zum-Fokussieren, soweit das Gerät es erlaubt. Ersatzweise „Foto scannen" oder manuelle Eingabe.
- **Raum-Logik:** Zuerst den Raum-QR scannen, dann die Gebinde. Steht ein Gebinde laut Liste in einem anderen Raum, warnt die App.
- **Rückmeldung nach jedem Scan:** grün (passt), orange (falscher Raum oder schon gescannt), rot (Id nicht in der Liste), dazu Ton und Vibration.
- **Sprachausgabe** (optional): Ansage von Name, Id oder nur Warnungen.
- **Abgleich:** Soll, gefunden und fehlend für den aktuellen Raum, dazu falsch platzierte und unbekannte Gebinde.
- **Export als CSV-Datei** (Semikolon-getrennt, direkt in Excel zu öffnen), alternativ in die Zwischenablage kopieren.
- **Liste hochladen:** Der Excel-Export des Bestands wird direkt auf dem Gerät geladen und nicht an einen Server gesendet.
- **Offline-fähig** als installierbare Web-App (PWA).

## Benutzung

1. App-Adresse auf dem Handy öffnen (Chrome auf Android, Safari auf dem iPhone) und zum Home-Bildschirm hinzufügen.
2. Tab **Liste**: Excel-Export des Bestands hochladen (Format siehe unten).
3. Tab **Scannen**: Kamera starten, Raum-QR scannen, dann die Gebinde des Raums.
4. Tab **Abgleich**: Sehen, was im Raum fehlt. Zum nächsten Raum wechseln, indem man dessen Raum-QR scannt.
5. Tab **Export**: Ergebnis als CSV speichern. Auf dem iPhone im Teilen-Menü „In Dateien sichern" wählen.

Beim ersten Start fragt der Browser nach der Kamera-Erlaubnis. Die Kamera funktioniert nur über HTTPS, was bei GitHub Pages der Fall ist.

## QR-Code-Formate

| Code | Inhalt | Beispiel |
|---|---|---|
| Gebinde | nur die Umgang-Id | `26271` |
| Raum | `RAUM:` + Liegenschaft-Haus-Raum-Nr, Großbuchstaben | `RAUM:FB-80-055` |

Der Raumschlüssel wird aus der hochgeladenen Liste gebildet (Spalten Liegenschaft, Haus und Raum-Nr, verbunden mit Bindestrichen). Der Vergleich ist exakt, `FB-80-55` passt also nicht zu `FB-80-055`.

## Format der Excel-Liste

Dateityp `.xlsx`, `.xls` oder `.csv`, gelesen wird das erste Tabellenblatt. Die Spalten werden an der Überschrift erkannt, Reihenfolge und weitere Spalten sind egal.

| Überschrift | Pflicht | Verwendung |
|---|---|---|
| `Umgang-Id` | ja | Inhalt des Gebinde-QR-Codes |
| `Handelsname` | ja | Anzeige beim Scannen |
| `Liegenschaft` | ja | Teil des Raumschlüssels |
| `Haus` | ja | Teil des Raumschlüssels |
| `Raum-Nr` | ja | Teil des Raumschlüssels |
| `Standort im Raum` | nein | Anzeige im Abgleich |
| `Erloschen` | nein | Zeilen mit Eintrag werden ausgeblendet |

Hinweis: Raumnummern mit führenden Nullen (z. B. `055`) müssen in Excel als Text gespeichert sein.

## Änderungen

### Version 1.3 (23.10.2026)
- **Befristung prüfen:** Gebinde mit Eintrag in der Spalte `Befristung` (auch `Ablaufdatum`, `Verfallsdatum`, `MHD`) werden beim Scannen geprüft. Die App meldet „Abgelaufen" (rot) oder „Läuft innerhalb eines Jahres ab" (orange), optisch und per Ansage.
- **Abgleich:** neue Liste „Befristung abgelaufen oder endet binnen 12 Monaten".
- **Vibration** bei jedem erkannten Code, mit unterschiedlichem Muster je nach Ergebnis (nur Android, auf dem iPhone nicht möglich).
- **CSV-Export:** zwei neue Spalten `Befristung` und `Ablauf`.
- **Update-Hinweis:** Nach dem Update die Excel-Liste erneut hochladen, damit die Befristung geladen wird.

### Version 1.2 (23.10.2026)
- CSV-Export als Datei (Semikolon, Excel-tauglich), „Kopieren" bleibt erhalten.
- „Fehlt" im Abgleich nur noch für den aktuellen Raum.

### Version 1.1 (01.10.2026)
- Versionsnummer im Kopf der App.
- Sprachausgabe (Name, Id oder nur Warnungen), neues Layout mit sichtbarem Ergebnis über dem Kamerabild.
- Zielrahmen, höhere Kameraauflösung und bessere Erkennung kleiner QR-Codes.

## Datenschutz

- Dieses Repository enthält **nur den Programmcode**, keine Chemikalien- oder Personendaten.
- Die hochgeladene Liste und alle Scans werden ausschließlich im Browser des jeweiligen Geräts gespeichert (localStorage) und verlassen es nicht.
- Daten sind pro Gerät und pro App-Adresse getrennt. Ändert sich die Adresse, sind sie auf dem Gerät nicht mehr sichtbar. Vor Änderungen deshalb die CSV exportieren.

## Grenzen

- Kein Abgleich zwischen mehreren Geräten. Bei mehreren Personen exportiert jedes Gerät seine eigene CSV, die zusammengeführt wird.
- Löscht man die Browserdaten, sind Liste und Scans weg. Zwischendurch exportieren.
- Kamera-Fokus und Zoom lassen sich im Browser nur auf manchen Geräten steuern (vor allem Android/Chrome).

## Dateien im Repository

| Datei | Zweck |
|---|---|
| `index.html` | die komplette App (Oberfläche und Logik) |
| `manifest.webmanifest` | Angaben zur Installation auf dem Home-Bildschirm |
| `sw.js` | Service Worker für den Offline-Betrieb |
| `icon-192.png`, `icon-512.png` | App-Symbole |

## Enthaltene Bibliotheken

- [jsQR](https://github.com/cozmo/jsQR) (Apache-2.0), QR-Erkennung
- [SheetJS Community Edition](https://github.com/SheetJS/sheetjs) 0.18.5 (Apache-2.0), Lesen von Excel-Dateien
