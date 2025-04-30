```package
iot-cube=github:Smartfeld/pxt-iot-cube#v1.1.2
sensors=github:Smartfeld/pxt-sensorikAktorikSmartfeld
```
### @explicitHints false

# Warteschlangen Tutorial - Teil 2

## 📗 Einführung,  Teil 2

**Vorraussetzungen**
🌱 IoT Basics abgeschlossen und Warteschlangen Tutorial [Teil 1 - noch ohne Internetverbindung](https://makecode.microbit.org/#tutorial:github:fave-smartfeld/pxt-smart-toilet-tutorial/docs/tutorials/warteschlange-part1) abgeschlossen.

Schwierigkeitsgrad: 🔥🔥⚪⚪

## 👁️ Vorraussetzungen @showdialog
* Schliesse den Cube so an, falls noch nicht gemacht:
![Bild](https://reifab.github.io/pxt-iot-tutorial/static/tutorials/iot-cube-anschliessen-klein.png)
* Stelle die Schalter vorerst so ein:
    * Battery Switch: **off**
    * LoRa Module: **on**
![Bild](https://reifab.github.io/pxt-iot-tutorial/static/tutorials/iot-cube-power-switches-klein.png)

* Ein LoRa-Gateway🛜 muss in Reichweite sein, welches mit TTN (The Things Network) verbunden ist.
Dies ist im Klassensatz einmal vorhanden und kann hunderte von IoT- Cubes bedienen.
![Bild](https://reifab.github.io/pxt-iot-tutorial/static/tutorials/gateway-klein.png)

## Lernergebnis

Aus dem Tutorial Teil 1 hast du bereits ein Programm, das die Anzahl an Personen in der Warteschlange mit den Knöpfen A und B setzt. 
Nun wollen wir die Anzahl an Personen in der Warteschlange über LoRa🛜 ins Internet senden. Am Ende hast du ein funktionsfähiges Programm, das...

* Eine LoRa-Verbindung🛜 aufbaut. 
* Die Anzahl an Personen in der Warteschlange 👥 über LoRa🛜 sendet. 

Das vollständige Programm aus Teil 1 ist bereits integriert. Falls dir etwas unklar ist, überlege nochmals den Teil 1 des Tutorial zu bearbeiten.




```template
function zeigePersonenanzahl () {
    basic.showNumber(anzahlPersonenInWarteschlange)
}
let anzahlPersonenInWarteschlange = 0
zeigePersonenanzahl()
basic.forever(function () {
    if (input.buttonIsPressed(Button.A)) {
        anzahlPersonenInWarteschlange += 1
        zeigePersonenanzahl()
    }
    if (input.buttonIsPressed(Button.B)) {
        anzahlPersonenInWarteschlange += 0 - 1
        if (anzahlPersonenInWarteschlange < 0) {
            anzahlPersonenInWarteschlange = 0
        }
        zeigePersonenanzahl()
    }
})
```
