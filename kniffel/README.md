# Kniffel

Ein Kniffel-Spiel (Yahtzee) für den Browser, gebaut mit React und Vite. 1 bis 4 Spieler wechseln sich am selben Gerät ab. Die Würfel sind 3D-Würfel, die beim Wurf wackeln und auf einer zufälligen Seite liegen bleiben.

## Voraussetzungen

- **Node.js** in einer aktuellen LTS-Version (von [nodejs.org](https://nodejs.org) oder unter Windows mit `winget install OpenJS.NodeJS.LTS`)
- Ein moderner Browser (Chrome, Edge, Firefox, Safari)

Prüfe die Installation in einem **neuen** Terminal:

```bash
node -v
npm -v
```

Beide Befehle sollten eine Versionsnummer anzeigen.

## Installation

Es gibt zwei Wege. Nimm Weg A, wenn du das Projekt von Grund auf neu aufsetzt, und Weg B, wenn du schon einen fertigen Projektordner hast.

### Weg A: Neues Vite-Projekt anlegen

```bash
npm create vite@latest kniffel -- --template react
cd kniffel
npm install
```

Danach ersetzt oder ergänzt du in `src/` diese drei Dateien:

| Datei | Inhalt |
| --- | --- |
| `src/App.jsx` | Oberfläche und Spielablauf |
| `src/App.css` | Gestaltung |
| `src/scoring.js` | Punkteberechnung (Spielregeln) |

Außerdem:

- Entferne in `src/main.jsx` die Zeile `import './index.css'`, falls sie dort steht. Die Datei `src/index.css` kannst du dann löschen.
- Der Ordner `src/assets` und die Datei `public/vite.svg` werden nicht gebraucht und können gelöscht werden.

### Weg B: Fertigen Projektordner verwenden

```bash
cd kniffel
npm install
```

## Spiel starten

```bash
npm run dev
```

Öffne dann die angezeigte Adresse im Browser, meistens <http://localhost:5173>. Der Server lädt die Seite bei jeder Änderung an einer Datei automatisch neu. Beenden kannst du ihn mit `Strg + C`.

## Spielen

1. Wähle die Spielerzahl und gib die Namen ein. Ein Klick in ein Namensfeld markiert den Standardnamen, sodass du ihn direkt überschreiben kannst.
2. Wer dran ist, würfelt bis zu dreimal. Nach dem ersten Wurf kannst du Würfel durch Anklicken festhalten. Festgehaltene Würfel werden nicht neu geworfen.
3. Trage dein Ergebnis in einer freien Kategorie ein. Die möglichen Punkte siehst du direkt in der Tabelle. Passt nichts, streichst du ein Feld mit 0 Punkten.
4. Danach ist der nächste Spieler dran. Das Spiel endet, wenn alle 13 Kategorien aller Spieler gefüllt sind.

### Punkte

| Kategorie | Punkte |
| --- | --- |
| Einser bis Sechser | Summe der Würfel mit dieser Augenzahl |
| Bonus | 35 Punkte, wenn die Summe von Einser bis Sechser mindestens 63 beträgt |
| Dreierpasch | Summe aller Würfel, wenn mindestens drei gleich sind |
| Viererpasch | Summe aller Würfel, wenn mindestens vier gleich sind |
| Full House | 25 Punkte für drei gleiche und zwei gleiche Würfel |
| Kleine Straße | 30 Punkte für vier aufeinanderfolgende Zahlen |
| Große Straße | 40 Punkte für fünf aufeinanderfolgende Zahlen |
| Kniffel | 50 Punkte für fünf gleiche Würfel |
| Chance | Summe aller Würfel |

Hinweis: Es gelten die strengen Regeln. Ein Kniffel zählt nicht als Joker für Full House oder die Straßen.

## Projektaufbau

```
kniffel/
├── index.html
├── package.json
└── src/
    ├── main.jsx      Einstiegspunkt
    ├── App.jsx       Oberfläche, Würfel-Animation, Spielablauf
    ├── App.css       Gestaltung
    └── scoring.js    Kategorien und Punkteberechnung
```

Die Spielregeln (`scoring.js`) sind bewusst von der Oberfläche (`App.jsx`) getrennt. Das macht es einfacher, später zum Beispiel einen Multiplayer-Modus anzubauen, bei dem ein Server würfelt und den Spielstand an alle Clients schickt.

## Für andere bereitstellen

```bash
npm run build
```

Der Befehl erzeugt den Ordner `dist/` mit einer fertigen Version. Du kannst ihn bei einem statischen Hoster wie Netlify, Vercel oder GitHub Pages hochladen. Mit `npm run preview` testest du die gebaute Version vorher lokal.

## Häufige Probleme

**„npm wurde nicht als Name eines Cmdlet … erkannt"**
Node.js ist nicht installiert, oder das Terminal wurde vor der Installation geöffnet. Installiere Node.js, schließe alle Terminal- und VS-Code-Fenster und öffne ein neues.

**„Die Ausführung von Skripts ist auf diesem System deaktiviert"** (Windows PowerShell)
Führe einmalig diesen Befehl aus und bestätige mit `J`:

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

Alternativ kannst du statt PowerShell die Eingabeaufforderung (`cmd`) verwenden.

**„Failed to resolve import …"**
Ein Importpfad stimmt nicht. In `src/App.jsx` muss `import { CATS, score, upperSum, bonus, total } from "./scoring";` stehen, und `scoring.js` muss direkt neben `App.jsx` liegen. Achte darauf, dass Windows die Datei nicht als `scoring.js.txt` gespeichert hat.

**Es sind nicht alle Würfelseiten sichtbar**
In `src/App.css` muss die Regel `.die` die Zeile `perspective: 400px;` enthalten, und `.dice` darf keine `perspective` haben.

**Der Port 5173 ist belegt**
Vite weicht automatisch auf einen anderen Port aus. Die richtige Adresse steht im Terminal.
