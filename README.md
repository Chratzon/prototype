# doorku Prototyp als PWA

Installierbare Web-App (Progressive Web App) für Handy und Tablet. Alle Daten bleiben lokal im Browser des Geräts (IndexedDB), es gibt keinen Server.

## Inhalt

| Datei | Zweck |
|---|---|
| `index.html` | Die komplette App (Übersicht als Startseite, Türliste nach Dringlichkeit sortiert, Türdetail mit Aufgabe/Priorität/Frist und Historie, Plan mit Pins, Tabelle, Projekte, Nutzerwechsel) |
| `manifest.webmanifest` | Name, Farben und Icons für „Zum Home-Bildschirm“ |
| `sw.js` | Service Worker: App funktioniert nach dem ersten Aufruf auch offline |
| `icons/` | App-Icons (SVG, 192/512 px, maskable, Apple Touch Icon) |
| `vendor/` | PDF.js 3.11.174 (Apache-2.0) zum Rendern hochgeladener PDF-Pläne, lokal mitgeliefert, damit es offline geht |

## Aufs Handy bringen

Eine PWA braucht eine https-Adresse. Am schnellsten:

1. Den ganzen Ordner `prototype-pwa` auf https://app.netlify.com/drop ziehen (oder in ein GitHub-Repo legen und GitHub Pages aktivieren).
2. Die Adresse auf dem Handy öffnen.
3. Android/Chrome: Hinweis „Installieren“ unten in der App oder Menü → „App installieren“.
   iPhone/iPad: In Safari auf Teilen → „Zum Home-Bildschirm“.

Lokal testen: `python3 -m http.server` im Ordner starten und http://localhost:8000 öffnen.

## Nach Änderungen

In `sw.js` die Konstante `VERSION` hochzählen, damit installierte Apps die neuen Dateien laden.

## Daten

Projekte → „Sicherung exportieren“ speichert alles (inkl. Pläne) als JSON-Datei, „Sicherung importieren“ stellt sie wieder her. Löscht man die Browserdaten, sind die Daten ohne Sicherung weg.

## Übersicht und Reihenfolge

Die App startet immer mit der Übersicht über alle Projekte: Kennzahlen, „Jetzt angehen“, anstehende Wartungen (60 Tage), Fristen, Projektstand und letzte Aktivität.

Jede Tür kann eine offene Aufgabe mit Priorität (Dringend, Hoch, Normal, Niedrig) und Frist haben. Türliste und Tabelle ordnen sich automatisch so: überfällige Frist oder Wartung, dann Dringend, dann Frist in 14 Tagen oder Wartung in 30 Tagen, dann Hoch, dann offene Mängel, dann Türen mit Termin vor Türen ohne Termin, erledigte Türen zuletzt. Innerhalb einer Stufe entscheidet Priorität, dann der nächste Termin.

## Projektphasen

Jedes Projekt hat eine Phase: Planung, Bauphase oder Nutzung. Gewählt wird sie im Projekt-Menü (oben auf den Projektnamen tippen) oder beim Anlegen. Wartungsfristen werden erst in der Phase „Nutzung“ überwacht. Die Übersicht lässt sich nach Phase filtern.
