# VFD-Simulator — Noritake Itron CU20045SCPB-W5J

Pixelmatrix-Simulator für das VFD-Textdisplay des Hardware-Gadgets.
Live: https://frankstuhr.github.io/web/VFD/

## Die Matrix

Am Foto der echten Röhre ausgemessen und mit der Typenbezeichnung abgeglichen
(CU**200**45 → 20 Zeichen, 04 Zeilen):

| | |
|---|---|
| Zeichen | 20 × 4 |
| Dots je Zeichen | 5 × 7 |
| Gesamtmatrix | **100 × 28 Dots** (2800 Stück) |
| Zeichensatz | HD44780-kompatibel, A00 |

Es gibt **keine achte Dot-Zeile**. Bei CGRAM-Zeichen (`createChar`) wird trotzdem
ein 8-Byte-Array erwartet — das achte Byte bleibt hier immer `0x00`.

## Bedienung

- **Text:** vier Felder à 20 Zeichen, werden direkt gerendert
- **Zeile 1 doppelt hoch:** rendert Zeile 1 in doppelter Größe über die
  Zeichenzeilen 1 und 2 — entspricht dem, was `LCDBigNumbers` auf der echten
  Hardware macht. Zeile 2 ist dann belegt.
- **Wie im Foto:** setzt den Zustand aus dem Referenzfoto (große Uhrzeit oben,
  Sensorwerte unten)
- **Dots setzen / löschen:** einzelne Dots anklicken oder mit gedrückter
  Maustaste ziehen
- **Export:** eine Zeichenzelle anklicken → fertiges `byte glyph[8]` samt
  `createChar`/`setCursor`-Aufruf. Alternativ das ganze Bild als ASCII-Art.

Achtung: Änderungen am Text bauen die Matrix neu auf und überschreiben eine
Zeichnung. „Aus Text neu aufbauen" macht das absichtlich.

## Dateien

| Datei | Inhalt |
|---|---|
| `index.html` | Simulator (Canvas-Rendering, keine Abhängigkeiten) |
| `font5x7.js` | 5×7-Zeichensatz A00, je Zeichen 7 Zeilen à 5 Bit |

Der Zeichensatz ist bewusst als lesbares Bit-Array abgelegt — neue Zeichen lassen
sich dort direkt ergänzen. Unbekannte Zeichen erscheinen als leeres Kästchen,
damit sofort auffällt, dass die Hardware sie nicht darstellen kann.
