```package
iot-cube=github:Smartfeld/pxt-iot-cube#v1.1.2
sensors=github:Smartfeld/pxt-sensorikAktorikSmartfeld
```
### @explicitHints false

# Smart Toilet Tutorial - Teil 1

## 📗 Einführung,  Teil 1

**Vorraussetzungen**
* Micro:Bit Basics: 
    * Du kannst Programme erstellen und herunterladen.
    * Du kennst die Einstiegspunkte "Beim Start" und "Dauerhaft".
    * Dir ist klar, dass Programme in der Regel schrittweise (von oben nach unten) abgearbeitet werden. Zudem kannst Du Schleifen und Verzweigungen einsetzen.
    * Es ist bekannnt, dass Kategorien einzelne Blöcke (z.B. ``||basic:Grundlagen||``) beinhalten, welche in Programmen genutzt werden können.
    * Variablen können erstellt, verwendet und verändert werden

Schwierigkeitsgrad: 🔥⚪⚪⚪

## Lernergebnis

In diesem Tutorial baust du Schritt für Schritt ein Programm auf, das einen Seifenstand simuliert und über 🛜 LoRa ins Internet sendet. Am Ende hast du ein funktionsfähiges Programm, das...

* den Status "Frei" / "Besetzt" einer Toilette 🚽 anzeigt.
* per ``||input:Knopfdruck||`` den Status anpasst:
    * ``||Input:Knopf A ist geklickt||``: Toilette 🚽 wird durch Knopf A betreten.
    * ``||Input:Knopf B ist geklickt||``: Toilette 🚽 wird durch Knopf B verlassen.

Wir starten langsam....

## 🚽 Variable für den Status der Toilette
Um den Status der Toilette zu speichern, nutzen wir eine Variable.
* ``||variables:Erstelle eine Variable...||`` und benenne sie mit **statusFreiOderBesetzt** 🚽.
* Die Toilette ist am Beginn frei. Setze deshalb ``||basic:beim Start||`` die zuvor angelegte Variable: ``||variables:setze statusFreiOderBesetzt auf 1||``🚽

```blocks
let statusFreiOderBesetzt = 0
statusFreiOderBesetzt += 1
```

## Symbol für den Status "Frei" ⬆️ der Toilette
Um den Status der Toilette anzuzeigen, nutzen wir die LED Matrix.
* Hol dir den Block ``||basic:Zeige LEDs|`` und ziehe diesen in den Block **beim Start** direkt unter die Variable **statusFreiOderBesetzt** 🚽.
* Aktiviere 9 LEDS, um einen Pfeil nach oben ⬆️ darzustellen, welcher symbolisiert, dass die Toilette frei ist. 
* 📥 Drücke `|Download|` und kontrolliere die LED- Anzeige:  
⬛⬛🟥⬛⬛   
⬛🟥🟥🟥⬛  
🟥⬛🟥⬛🟥  
⬛⬛🟥⬛⬛  
⬛⬛🟥⬛⬛   
Wird der Pfeil dargestellt?

```blocks
let statusFreiOderBesetzt = 0
statusFreiOderBesetzt += 1
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
* Hol dir den Block ``||functions:Erstelle eine Funktion...|`` und benenne die Funktion "macheFrei".
* Nimm die beiden zuvor angelegten Schritte  aus dem "beim Start" und ziehe diese in die Funktion. 
* Hol dir den Block ``||functions:Aufruf macheFrei |`` und ziehe diesen in den Block **beim Start**.
* 📥 Drücke `|Download|` und kontrolliere die LED- Anzeige:  
⬛⬛🟥⬛⬛   
⬛🟥🟥🟥⬛  
🟥⬛🟥⬛🟥  
⬛⬛🟥⬛⬛  
⬛⬛🟥⬛⬛   
Wird der Pfeil noch immer dargestellt?

```blocks
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
macheFrei()
```

## Status der Toilette mit Knopf A auf "Besetzt" setzen  
Um den Status der Toilette auf "Besetzt" zu setzen, nutzen wir ``||input:Knopf A ist geklickt||``:
* Hole dir den  ``||input:Knopf A ist geklickt||`` Block. 
* Die Toilette ist jetzt besetzt: ``||variables:setze statusFreiOderBesetzt||``auf 0 🚽.
* Hol dir den Block ``||basic:Zeige LEDs|`` und ziehe diesen in den Block **Wenn Knopf A geklickt ** direkt unter die Variable **statusFreiOderBesetzt** 🚽.
* Aktiviere die 11 LEDS, um die besetzte Toilette 🚽 zu symbolisieren. 
* 📥 Drücke `|Download|` und kontrolliere die LED- Anzeige, wenn du Knopf A gedrückt hast:  
⬛⬛⬛⬛🟥   
⬛⬛⬛⬛🟥  
⬛⬛⬛⬛🟥  
🟥🟥🟥🟥🟥  
⬛🟥🟥🟥⬛   
Wird das Symbol dargestellt?

```blocks
input.onButtonPressed(Button.A, function () {
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
* Hol dir den Block ``||functions:Erstelle eine Funktion...|`` und benenne die Funktion "mache besetzt".
* Nimm die beiden zuvor angelegten Schritte aus dem "Wenn Knopf A geklickt" und ziehe diese in die Funktion.
* Hol dir den Block ``||functions:Aufruf macheBesetzt |`` und ziehe diesen in den Block **Wenn Kopf A geklickt**.
* 📥 Drücke `|Download|` und kontrolliere die LED- Anzeige, wenn du Knopf B gedrückt hast:  
⬛⬛⬛⬛🟥   
⬛⬛⬛⬛🟥  
⬛⬛⬛⬛🟥  
🟥🟥🟥🟥🟥  
⬛🟥🟥🟥⬛   
Wird das Symbol noch immer dargestellt?

```blocks
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
    macheBesetzt()
})
let statusFreiOderBesetzt = 0
```

## Status der Toilette mit Knopf B auf "Frei" setzen  
Um den Status der Toilette auf "Frei" zu setzen, nutzen wir ``||input:Knopf A ist geklickt||``:
* Hole dir den  ``||input:Knopf A ist geklickt||`` Block. 
* Ändere "A" auf "B", damit du auf den Knopf "B" reagieren kannst: ``||input:Knopf B ist geklickt||``
* Hol dir den Block ``||functions:Aufruf macheFrei |`` und ziehe diesen in den Block **Wenn Kopf B geklickt**.
* 📥 Drücke `|Download|` und kontrolliere die LED- Anzeige, wenn du Knopf A 🚽 und dann B ⬆️ gedrückt hast:  
⬛⬛🟥⬛⬛     ⬛⬛⬛⬛🟥     ⬛⬛🟥⬛⬛ 
⬛🟥🟥🟥⬛     ⬛⬛⬛⬛🟥     ⬛🟥🟥🟥⬛
🟥⬛🟥⬛🟥     ⬛⬛⬛⬛🟥     🟥⬛🟥⬛🟥 
⬛⬛🟥⬛⬛     🟥🟥🟥🟥🟥     ⬛⬛🟥⬛⬛  
⬛⬛🟥⬛⬛     ⬛🟥🟥🟥⬛     ⬛⬛🟥⬛⬛  
Werden dir die korrekten Symbole für den Status der Toilette angezeigt?

```blocks
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
input.onButtonPressed(Button.B, function () {
    macheFrei()
})
let statusFreiOderBesetzt = 0
macheFrei()
```

## Weiter gehts mit Teil 2!
Im nächsten Teil Status der Toilett an die Claviscloud senden 

* [Teil 2](https://makecode.microbit.org/#tutorial:github:fave-smartfeld/pxt-smart-toilet-tutorial/docs/tutorials/smart-toilet-part2)
* Bitte überprüfe zuvor, ob dein Programm funktioniert!
* 📥 Drücke `|Download|` und kontrolliere die LED- Anzeige, wenn du Knopf A und dann B gedrückt hast:  
⬛⬛🟥⬛⬛     ⬛⬛⬛⬛🟥     ⬛⬛🟥⬛⬛ 
⬛🟥🟥🟥⬛     ⬛⬛⬛⬛🟥     ⬛🟥🟥🟥⬛
🟥⬛🟥⬛🟥     ⬛⬛⬛⬛🟥     🟥⬛🟥⬛🟥 
⬛⬛🟥⬛⬛     🟥🟥🟥🟥🟥     ⬛⬛🟥⬛⬛  
⬛⬛🟥⬛⬛     ⬛🟥🟥🟥⬛     ⬛⬛🟥⬛⬛  
Werden dir die korrekten Symbole für den Status der Toilette angezeigt?

Klicke auf das 💡- Symbol, um das vollständige Programm anzuzeigen.

```blocks
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
    macheBesetzt()
})
input.onButtonPressed(Button.B, function () {
    macheFrei()
})
let statusFreiOderBesetzt = 0
macheFrei()
```