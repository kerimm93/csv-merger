# CSV Merger

Kleine, lokal ausführbare Single-File-Webapp zum Zusammenführen vieler CSV-Dateien.  
Die App gruppiert Dateien automatisch nach identischem Header und kann jede Gruppe als neue CSV exportieren. Zusätzlich können Gruppen mit unterschiedlichen Headern über eine Spalten-Union in eine gemeinsame Datei überführt werden.

## Wofür ist die App gedacht?

CSV Merger ist praktisch, wenn viele kleine CSV-Dateien aus Exporten, Formularen, Tools oder Tabellen zusammengeführt werden sollen, ohne dafür Excel, LibreOffice oder ein Skript zu öffnen.

Typische Anwendungsfälle:

- mehrere CSV-Exports mit gleichem Spaltenaufbau zu einer Datei zusammenführen
- schnell sehen, welche Dateien denselben Header haben
- Dateien mit unterschiedlichen Headern kontrolliert in eine gemeinsame CSV-Struktur bringen
- lokale Verarbeitung sensibler Daten ohne Upload zu einem externen Dienst

## Features

- **Drag-and-drop Upload** für mehrere Dateien
- **Dateiauswahl per Klick** als Alternative zum Drag-and-drop
- **Automatische Delimiter-Erkennung** für:
  - Semikolon
  - Komma
  - Tab
  - Pipe
- **CSV-Parsing mit Quote-Unterstützung** nach dem üblichen RFC-4180-Prinzip:
  - Felder in Anführungszeichen
  - doppelte Anführungszeichen innerhalb von Feldern
  - Zeilenumbrüche innerhalb quotierter Felder
- **Automatische Gruppierung nach Header**
- **Merge pro Header-Gruppe**
- **Union-Merge für unterschiedliche Header**
  - bildet eine gemeinsame Spaltenmenge
  - fehlende Spalten werden leer gefüllt
  - stabile Spaltenreihenfolge
- **Optionaler UTF-8-BOM beim Export**
- **Keine Installation, keine Abhängigkeiten, kein Build-Step**
- **Läuft vollständig im Browser**

## Datenschutz

Die App verarbeitet Dateien ausschließlich lokal im Browser.  
Es gibt keinen Server, keine Datenbank, keinen externen Upload und keine Tracking-Integration.

Wichtig: Die Daten werden während der Nutzung im Browser-Speicher der geöffneten Seite gehalten und beim Leeren oder Neuladen verworfen. Exportierte Dateien werden nur erzeugt, wenn du aktiv auf einen Download-Button klickst.

## Nutzung

### 1. App öffnen

Repository klonen oder die Datei herunterladen:

```bash
git clone <REPOSITORY-URL>
cd <REPOSITORY-NAME>
```

Dann die HTML-Datei direkt im Browser öffnen:

```bash
open csv-merger.html
```

Unter Windows kann die Datei auch einfach per Doppelklick geöffnet werden.

### 2. CSV-Dateien hinzufügen

Ziehe mehrere CSV-Dateien in die Upload-Fläche oder klicke auf die Fläche, um Dateien auszuwählen.

Unterstützte Dateitypen:

- `.csv`
- `.tsv`
- `.txt`
- `text/csv`

### 3. Header-Gruppen prüfen

Die App erkennt die Header-Zeile jeder Datei und gruppiert Dateien mit identischem Header automatisch.  
Für jede Gruppe werden angezeigt:

- Anzahl der Spalten
- Anzahl der Dateien
- Anzahl der Datenzeilen
- erkannter Delimiter
- Header-Vorschau
- Liste der enthaltenen Dateien

### 4. Einzelne Gruppen zusammenführen

Klicke bei einer Gruppe auf:

```text
Gruppe X zusammenführen ↓
```

Die App erzeugt daraus eine neue CSV-Datei, zum Beispiel:

```text
merge_gruppe_1.csv
```

### 5. Unterschiedliche Header zusammenführen

Wenn mehrere Header-Gruppen vorhanden sind, erscheint ein zusätzlicher Bereich:

```text
Gruppen mit unterschiedlichen Headern vereinen
```

Dort kannst du mehrere Gruppen auswählen und einen Ausgabe-Delimiter festlegen.  
Die App erzeugt dann eine gemeinsame Datei:

```text
union_merge.csv
```

Dabei werden alle Spalten aus den ausgewählten Gruppen übernommen. Wenn eine Datei eine bestimmte Spalte nicht besitzt, bleibt das Feld in dieser Zeile leer.

## Technische Details

Die App besteht aus einer einzigen HTML-Datei mit eingebettetem CSS und JavaScript.

### Zentrale Logik

- `detectDelim(text)` erkennt den wahrscheinlichsten Delimiter anhand der ersten Zeile.
- `parseCSV(text, delim)` parst CSV-Daten inklusive Quotes und escaped Quotes.
- `buildGroups()` gruppiert geladene Dateien nach identischem Header.
- `mergeGroup(group)` führt alle Zeilen einer Header-Gruppe zusammen.
- `unionMerge(selectedGroups, delim)` erzeugt eine gemeinsame Spalten-Union über mehrere Gruppen hinweg.
- `toCSV(header, rows, delim)` serialisiert die Daten wieder als CSV.
- `download(filename, csvText)` erzeugt lokal eine Download-Datei über `Blob` und `URL.createObjectURL`.

## Projektstruktur

```text
.
├── csv-merger.html
└── README.md
```

## Entwicklung

Für die lokale Entwicklung ist kein Setup nötig.  
Änderungen können direkt in `csv-merger.html` vorgenommen und anschließend durch Neuladen im Browser getestet werden.

Optional kann ein lokaler statischer Server genutzt werden:

```bash
python -m http.server 8000
```

Danach im Browser öffnen:

```text
http://localhost:8000/csv-merger.html
```

## Grenzen und Hinweise

- Die App arbeitet im Browser-Speicher. Sehr große Dateien können je nach Gerät und Browser langsam werden oder Speicherprobleme verursachen.
- Die Delimiter-Erkennung basiert auf der ersten Zeile. Bei ungewöhnlichen oder defekten Dateien sollte das Ergebnis geprüft werden.
- Header werden exakt verglichen. Unterschiedliche Schreibweisen, Leerzeichen oder Reihenfolgen erzeugen unterschiedliche Gruppen.
- Es findet keine automatische Dublettenprüfung statt.
- Es findet keine automatische Datentyp-Erkennung oder Bereinigung statt.

## Mögliche Weiterentwicklungen

- manuelle Delimiter-Auswahl pro Datei
- Header-Normalisierung, zum Beispiel Trim, Kleinschreibung oder Mapping
- Vorschau der ersten Datenzeilen
- Dublettenprüfung
- Spaltenauswahl vor dem Export
- Export als ZIP für mehrere Gruppen
- Fortschrittsanzeige bei sehr vielen Dateien
- Tests für Parser, Delimiter-Erkennung und Union-Merge



