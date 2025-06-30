```package
iot-cube=github:Smartfeld/pxt-iot-cube#v1.1.2
sensors=github:Smartfeld/pxt-sensorikAktorikSmartfeld
```
### @explicitHints false

# Smart Toilet Tutorial - Teil 1

## 📗 Einführung, Teil 1

**Voraussetzungen**
* Micro:Bit Basics: 
    * Du kannst Programme erstellen und herunterladen.
    * Du kennst die Einstiegspunkte "Beim Start" und "Dauerhaft".
    * Dir ist klar, dass Programme in der Regel schrittweise (von oben nach unten) abgearbeitet werden. Zudem kannst du Schleifen und Verzweigungen einsetzen.
    * Es ist bekannt, dass Kategorien einzelne Blöcke (z.B. ``||basic:Grundlagen||``) beinhalten, welche in Programmen genutzt werden können.
    * Variablen können erstellt, verwendet und verändert werden.

Schwierigkeitsgrad: 🔥⚪⚪⚪

## 👁️ Voraussetzungen @showdialog
* Für Teil 1 brauchst du grundsätzlich nur einen Micro:Bit. 
* Falls du lieber gleich den IoT- Cube nehmen möchtest, kannst du ihn so anschliessen. Achte auf
die rote Markierung am USB Kabel und schliesse es wie folgt an:
![Bild](https://reifab.github.io/pxt-iot-tutorial/static/tutorials/iot-cube-anschliessen-klein.png)
* Stelle die Schalter vorerst so ein:
    * Battery Switch: **off**
    * LoRa Module: **on**
![Bild](https://reifab.github.io/pxt-iot-tutorial/static/tutorials/iot-cube-power-switches-klein.png)
* Überprüfe, ob der micro:bit verbunden ist.

## Lernergebnis

In diesem Tutorial baust du Schritt für Schritt ein Programm auf, das den Status einer Toilette simuliert. Am Ende hast du ein funktionsfähiges Programm, das ...

* den Status "Frei"/"Besetzt" einer Toilette 🚽 anzeigt.
* per ``||input:Knopfdruck||`` den Status anpasst:
    * ``||Input:Knopf A ist geklickt||``: Toilette 🚽 wird durch Knopf A betreten.
    * ``||Input:Knopf B ist geklickt||``: Toilette 🚽 wird durch Knopf B verlassen.

Wir starten langsam ...

## 🚽 Variable für den Status der Toilette
Um den Status der Toilette zu speichern, nutzen wir eine Variable.
* ``||variables:Erstelle eine Variable ...||`` und benenne sie mit **statusFreiOderBesetzt** 🚽.
* Die Toilette ist am Beginn frei. Setze deshalb ``||basic:beim Start||`` die zuvor angelegte Variable: ``||variables:setze statusFreiOderBesetzt auf 1||``🚽

```blocks
let statusFreiOderBesetzt = 0
// @highlight
statusFreiOderBesetzt += 1
```

## Symbol für den Status "Frei" ⬆️ der Toilette
Um den Status der Toilette anzuzeigen, nutzen wir die LED-Matrix.
* Hol dir den Block ``||basic:Zeige LEDs|`` und ziehe diesen in den Block **beim Start** direkt unter die Variable **statusFreiOderBesetzt** 🚽.
* Aktiviere 9 LEDS, um einen Pfeil nach oben ⬆️ darzustellen, welcher symbolisiert, dass die Toilette frei ist. 
* 📥 Drücke `|Download|` und kontrolliere die LED-Anzeige:  
⬛⬛🟥⬛⬛   
⬛🟥🟥🟥⬛  
🟥⬛🟥⬛🟥  
⬛⬛🟥⬛⬛  
⬛⬛🟥⬛⬛   
Wird der Pfeil dargestellt?

```blocks
let statusFreiOderBesetzt = 0
statusFreiOderBesetzt += 1
// @highlight
basic.showLeds(`
    . . # . .
    . # # # .
    # . # . #
    . . # . .
    . . # . .
    `)
```

## Funktion für das Setzen des Status "Frei" ⬆️ der Toilette
Da wir später (Knopf B) auch den Status der Toilette setzen, nutzen wir eine Funktion für diese Aufgabe.
* Hol dir den Block aus Fortgeschritten - Funktionen ``||functions:Erstelle eine Funktion ...|`` und benenne die Funktion **macheFrei**.
* Nimm die beiden zuvor erstellten Befehle aus dem Start Block und ziehe sie in die neue Funktion. 
* Hol dir den Block aus Fortgeschritten - Funktionen ``||functions:Aufruf macheFrei |`` und ziehe diesen in den Block **beim Start**.
* 📥 Drücke `|Download|` und kontrolliere die LED-Anzeige:  
⬛⬛🟥⬛⬛   
⬛🟥🟥🟥⬛  
🟥⬛🟥⬛🟥  
⬛⬛🟥⬛⬛  
⬛⬛🟥⬛⬛   
Wird der Pfeil noch immer dargestellt?

```blocks
// @highlight
function macheFrei () {
    statusFreiOderBesetzt += 1
    basic.showLeds(`
        . . # . .
        . # # # .
        # . # . #
        . . # . .
        . . # . .
        `)
}
let statusFreiOderBesetzt = 0
// @highlight
macheFrei()
```

## Status der Toilette mit Knopf A auf "Besetzt" setzen  
Um den Status der Toilette auf "Besetzt" zu setzen, nutzen wir ``||input:Knopf A ist geklickt||``:
* Hole dir den  ``||input:Knopf A ist geklickt||`` Block. 
* In die Bedingung "Wenn Knopf A geklickt" setzen wir die Variable ``||variables:setze statusFreiOderBesetzt||``auf 0 🚽.
* Hol dir den Block ``||basic:Zeige LEDs|`` und ziehe diesen in den Block **Wenn Knopf A geklickt** direkt unter die Variable **statusFreiOderBesetzt** 🚽.
* Aktiviere die 11 LEDS, um die besetzte Toilette 🚽 zu symbolisieren. 
* 📥 Drücke `|Download|` und kontrolliere die LED-Anzeige, wenn du Knopf A gedrückt hast:  
⬛⬛⬛⬛🟥   
⬛⬛⬛⬛🟥  
⬛⬛⬛⬛🟥  
🟥🟥🟥🟥🟥  
⬛🟥🟥🟥⬛   
Wird das Symbol dargestellt?

```blocks
// @highlight
input.onButtonPressed(Button.A, function () {
    // @highlight
    statusFreiOderBesetzt = 0
    basic.showLeds(`
        . . . . #
        . . . . #
        . . . . #
        # # # # #
        . # # # .
        `)
})
let statusFreiOderBesetzt = 0
```

## Funktion für das Setzen des Status "Besetzt" 🚽 der Toilette
Ähnlich zur Funktion **macheFrei** erstelle eine Funktion für **macheBesetzt** und nutze diese.
* Hol dir den Block ``||functions:Erstelle eine Funktion ...|`` und benenne die Funktion **macheBesetzt**.
* Nimm die beiden zuvor angelegten Schritte aus dem "Wenn Knopf A geklickt" und ziehe diese in die Funktion.
* Hol dir den Block ``||functions:Aufruf macheBesetzt |`` und ziehe diesen in den Block **Wenn Kopf A geklickt**.
* 📥 Drücke `|Download|` und kontrolliere die LED-Anzeige, wenn du Knopf B gedrückt hast:  
⬛⬛⬛⬛🟥   
⬛⬛⬛⬛🟥  
⬛⬛⬛⬛🟥  
🟥🟥🟥🟥🟥  
⬛🟥🟥🟥⬛   
Wird das Symbol noch immer dargestellt?

```blocks
// @highlight
function macheBesetzt () {
    statusFreiOderBesetzt = 0
    basic.showLeds(`
        . . . . #
        . . . . #
        . . . . #
        # # # # #
        . # # # .
        `)
}
input.onButtonPressed(Button.A, function () {
    // @highlight
    macheBesetzt()
})
let statusFreiOderBesetzt = 0
```

## Status der Toilette mit Knopf B auf "Frei" setzen  
Um den Status der Toilette auf "Frei" zu setzen, nutzen wir ``||input:Knopf B ist geklickt||``:
* Hole dir den  ``||input:Knopf A ist geklickt||`` Block. 
* Ändere "A" auf "B", damit du auf den Knopf "B" reagieren kannst: ``||input:Knopf B ist geklickt||``
* Hol dir den Block ``||functions:Aufruf macheFrei |`` und ziehe diesen in den Block **Wenn Kopf B geklickt**.
* 📥 Drücke `|Download|` und kontrolliere die LED-Anzeige, nach dem Einschalten ⬆️, wenn du Knopf A 🚽 und dann B ⬆️ gedrückt hast. 

Werden dir die korrekten Symbole für den Status der Toilette angezeigt?

```blocks
// @hide
function macheFrei () {
    statusFreiOderBesetzt += 1
    basic.showLeds(`
        . . # . .
        . # # # .
        # . # . #
        . . # . .
        . . # . .
        `)
}
// @highlight
input.onButtonPressed(Button.B, function () {
    macheFrei()
})
let statusFreiOderBesetzt = 0
macheFrei()
```

## Gratuliere 🏆 - du hast den Teil 1 erfolgreich bearbeitet 🚀

Im nächsten Teil werden wir den Status der Toilette an die Claviscloud senden.

* Weiter gehts mit Teil 2: [Teil 2](https://makecode.microbit.org/#tutorial:github:fave-smartfeld/pxt-smart-toilet-tutorial/docs/tutorials/smart-toilet-part2)
* Falls irgendwas noch nicht richtig läuft, hier hast Du eine funktionierende Version zum testen: [Lösung Teil 1](https://makecode.microbit.org/#tutorial:github:fave-smartfeld/pxt-smart-toilet-tutorial/docs/tutorials/smart-toilet-part1-solution)