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

Die App startet immer mit der Übersicht über alle Projekte. Auf der Übersicht gibt es keine Sprünge in einzelne Projekte über die untere Leiste; dort wählt man nur zwischen den beiden Varianten (am Tablet oben rechts):

- **Nach Projekten:** je Projekt eine Karte mit Phase, Stand, Warnhinweisen und den Türen, die jetzt angegangen werden müssen; „Projekt öffnen“ wechselt ins Projekt.
- **Gesamt:** alle Projekte zusammen: „Jetzt angehen“, anstehende Wartungen (60 Tage), Fristen, Projektstand und letzte Aktivität.

Beide Varianten sind filterbar: Suche (Tür-Nr., Raum, Aufgabe, Brandschutz …), Projekt, Phase und die Kennzahlen (Überfällig, Dringend, Bald fällig, Offene Mängel lassen sich antippen). „Filter zurücksetzen“ hebt alles auf. Die gewählte Variante merkt sich die App.

Jede Tür kann eine offene Aufgabe mit Priorität (Dringend, Hoch, Normal, Niedrig) und Frist haben. Türliste und Tabelle ordnen sich automatisch so: überfällige Frist oder Wartung, dann Dringend, dann Frist in 14 Tagen oder Wartung in 30 Tagen, dann Hoch, dann offene Mängel, dann Türen mit Termin vor Türen ohne Termin, erledigte Türen zuletzt. Innerhalb einer Stufe entscheidet Priorität, dann der nächste Termin.

## Projektphasen

Jedes Projekt hat eine Phase: Planung, Bauphase oder Nutzung. Gewählt wird sie im Projekt-Menü (oben auf den Projektnamen tippen) oder beim Anlegen. Wartungsfristen werden erst in der Phase „Nutzung“ überwacht. Die Übersicht lässt sich nach Phase filtern.

Die App stellt je Phase andere Inhalte nach vorn:

| Phase | Startansicht | Im Vordergrund |
|---|---|---|
| Planung | Türliste | Planungsangaben mit Vollständigkeit (Türtyp, Brandschutz, Maße, Hersteller, Beschlag), Detailzeichnungen und Unterlagen je Tür (PDF oder Bild) |
| Bauphase | Plan | Teileliste je Tür (Offen, Bestellt, Geliefert, Montiert), Ansicht „Teile“ nach Einbauort, Teile im Plan-Pin |
| Nutzung | Wartung | Zustand (Gut bis Mangelhaft), fällige Wartungen, Wartungsprotokoll mit Datum, Zustand und Bemerkung |

## Dokumentation und Anbauteile

Jedes Projekt hat einen Bereich „Dokumente“ für die Unterlagen an den Bauherrn: Zulassungen, Übereinstimmungserklärungen, Prüfbücher, Datenblätter und Sonstiges (PDF oder Bild, Original bleibt erhalten, Vorschau und Download in der App).

- Ein Dokument kann beliebig vielen Türen zugeordnet werden (im Dokument per Filter, z. B. „T30“, und „Alle Treffer zuordnen“, oder in der Tür über „Dokumente zuordnen“).
- Anbauteile (Obentürschließer, Feststellanlage, Schloss, Profilzylinder …) sind eigene Objekte je Projekt mit eigenen Unterlagen. Ein Datenblatt am Anbauteil gilt automatisch für alle Türen, an denen es verbaut ist.
- Offene Punkte für die Übergabe: Brandschutztüren ohne Zulassung, Anbauteile ohne Datenblatt, abgelaufene und nicht zugeordnete Dokumente.

### Türliste in Tabellenform (Tabelle → „Erfassen“)

Editierbare Tabelle: Türen und Anbauteile direkt in Zellen anlegen, jede Änderung wird protokolliert. In Anbauteil-Spalten den Produktnamen eintragen; Unbekanntes wird als neues Anbauteil angelegt, mehrere mit „;“ trennen. Über „+ Anbauteil-Spalte“ kommen weitere Kategorien dazu. Zeilen aus Excel lassen sich einfügen (fehlende Türen werden angelegt). Mehrere Zeilen auswählen und gemeinsam Dokumente oder Anbauteile zuordnen. Der CSV-Export enthält Anbauteile und Dokumente je Tür.
