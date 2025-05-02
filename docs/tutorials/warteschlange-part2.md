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

Aus dem Tutorial Teil 1 hast du bereits ein Programm mit dem die Anzahl an Personen in einer Warteschlange mit den Knöpfen A und B verändert werden kann. 
Nun wollen wir die Anzahl an Personen in der Warteschlange über LoRa🛜 ins Internet senden. Am Ende hast du ein funktionsfähiges Programm, das...

* Eine LoRa-Verbindung🛜 aufbaut. 
* Die Anzahl an Personen in der Warteschlange 👥 über LoRa🛜 sendet. 

Das vollständige Programm aus Teil 1 ist bereits integriert. Falls dir etwas unklar ist, überlege nochmals den Teil 1 des Tutorial zu bearbeiten.

## 🛜 Verbindung mit Internet aufbauen - Schritt 1

Am Beginn bauen wir eine Verbindung zum Internet auf. 
Da wir bei dieser Aufgabe einige Anweisungen/Blöcke dazu benötigen, erstellen wir zuerst eine Funktion **initialisiereLoRaVerbindung**:
* Hol dir den Block ``||functions:Erstelle eine Funktion...||`` und benenne die Funktion **initialisiereLoRaVerbindung**.
* Hol dir den Block ``||functions:Aufruf initialisiereLoRaVerbindung ||`` und ziehe diesen an den Beginn des Blocks **beim Start**.

```blocks
function initialisiereLoRaVerbindung() {
}
initialisiereLoRaVerbindung()
anzahlPersonenInWarteschlange = 0
zeigePersonenanzahl()
```

## 🛜 Verbindung mit Internet aufbauen - Schritt 2

Zuerst integrieren wir alle Schritte zum Verbindungsaufbau in die Funktion **initialisiereLoRaVerbindung**. 
Im nächsten Schritt zeigen wir den Status des Verbindungsaufbaus auf dem OLED an.
* Ziehe den Block 🛜``||IoTCube:LoRa Netzwerk-Verbindung||`` in 
``||functions:initialisiereLoRaVerbindung||``.
* Ziehe darunter den Block ``||loops:während falsch mache||`` hinein. Weil
das Verbinden je nach Umständen 5 is 30 Sekunden dauert, wollen wir in dieser
Schleife verbleiben, solange die Verbindung noch **nicht** besteht.  
* Ziehe dazu den Block ``||Logic:nicht||`` auf die Schleife,
um den Wahrheitswert zu negieren.
* Füge in den **nicht** Block nun 🛜``||IoTCube:Lese Gerätestatus-Bit||`` ein.
Ändere darin das Gerätestatus-Bit auf **Verbunden**. Der Code in der Schleife lautet nun "während nicht Lese gerätestatus-Bit verbunden". 
Programmierer/innen lesen den Code so: "Während das Gerät nicht verbunden ist." 
* Warte in der Schleife 1 Sekunde (1000 ms). Nutze ``||basic:pausiere (ms)||``.

```blocks
function initialisiereLoRaVerbindung() {
    IoTCube.LoRa_Join(
    eBool.enable,
    eBool.enable,
    10,
    8
    )
    while (!(IoTCube.getStatus(eSTATUS_MASK.JOINED))) {
        basic.pause(1000)
    }
}
initialisiereLoRaVerbindung()
anzahlPersonenInWarteschlange = 0
zeigePersonenanzahl()
```

## 🛜 Verbindung mit Internet aufbauen - Schritt 3

Abschliessend möchten wir den Status des Verbindungsaufbaus auf dem OLED anzeigen.
* Ziehe den Block ``||smartfeldAktoren:Display:init OLED||`` an den Beginn der Funktion 
``||functions:initialisiereLoRaVerbindung||``. Belasse die Breite mit 128 und Höhe mit 64.
* Direkt darunter platziere den Block ``||smartfeldAktoren:Display:Lösche Displayinhalt||``, um den Inhalt des Displays zu löschen.
* Nutze im Anschluss den Block ``||smartfeldAktoren:Display:schreibe String||`` **"Verbinde"**, um zu zeigen, dass sich der IoT Cube versucht zu verbinden.
* Am Ende der Schleife **Während nicht "Verbunden"** nutze den Block ``||smartfeldAktoren:Display:schreibe String||`` **"."**, um jede Sekunde einen weiteren Punkt am Display anzuzeigen. Das verdeutlicht, dass der Verbindungsaufbau läuft.
* 📥 Drücke `|Download|` und kontrolliere das OLED-Display:  
Wird dir am Beginn "Verbinde" angezeigt? Wird jede Sekunde ein weiterer "." angezeigt? <br />
Werden irgenwann keine weiteren "." mehr angezeigt? <br />
Super, dann hat sich dein IoT Cube erfolgreich mit dem LoRa-Gateway 🛜 verbunden 🚀.

```blocks
function initialisiereLoRaVerbindung() {
    smartfeldAktoren.oledInit(128, 64)
    smartfeldAktoren.oledClear()
    smartfeldAktoren.oledWriteStr("Verbinde")
    IoTCube.LoRa_Join(
    eBool.enable,
    eBool.enable,
    10,
    8
    )
    while (!(IoTCube.getStatus(eSTATUS_MASK.JOINED))) {
        basic.pause(1000)
        smartfeldAktoren.oledWriteStr(".")
    }
}
let anzahlPersonenInWarteschlange = 0
initialisiereLoRaVerbindung()
anzahlPersonenInWarteschlange = 0
zeigePersonenanzahl()
```

## 🛜 Verbindungsbestätigung anzeigen

Abschliessend möchten wir den erfolgreichen Verbindungsaufbaus kurz auf dem OLED anzeigen.
* Ziehe dazu den Block ``||smartfeldAktoren:Display:Lösche Displayinhalt||`` ans Ende der Funktion bzw. nachdem die Schleife beendet ist, um den Inhalt des Displays zu löschen.
* Im Anschluss ziehe den Block ``||smartfeldAktoren:Display:schreibe String||`` **"Verbunden!"**, um zu zeigen, dass sich der IoT Cube erfolgreich verbunden hat.
* Um Strom zu sparten, löschen wir nach 2 Sekunden das Display wieder. Nutze dazu den Block ``||basic:pausiere (ms)||`` **2000ms** und ``||smartfeldAktoren:Display:Lösche Displayinhalt||``.
* 📥 Drücke `|Download|` und kontrolliere das OLED-Display:  
Wird dir nach erfolgreichem Verbindungsaufbau 2 Sekunden lang "Verbunden" anzgezeigt? <br />
Wird im Anschluss die Anzeige wieder gelöscht?

```blocks
function initialisiereLoRaVerbindung() {
    smartfeldAktoren.oledInit(128, 64)
    smartfeldAktoren.oledClear()
    smartfeldAktoren.oledWriteStr("Verbinde")
    IoTCube.LoRa_Join(
    eBool.enable,
    eBool.enable,
    10,
    8
    )
    while (!(IoTCube.getStatus(eSTATUS_MASK.JOINED))) {
        basic.pause(1000)
        smartfeldAktoren.oledWriteStr(".")
    }
    smartfeldAktoren.oledClear()
    smartfeldAktoren.oledWriteStr("Verbunden!")
    basic.pause(2000)
    smartfeldAktoren.oledClear()
}
let anzahlPersonenInWarteschlange = 0
initialisiereLoRaVerbindung()
anzahlPersonenInWarteschlange = 0
zeigePersonenanzahl()
```

## 👥 Senden der Daten in der Funktion "zeigePersonenanzahl" - Teil 1

ToDo.
* ToDo 

```blocks
function sendeUndZeigePersonenanzahl () {
    IoTCube.addUnsignedInteger(eIDs.ID_0, anzahlPersonenInWarteschlange)
    IoTCube.SendBufferSimple()
    basic.showNumber(anzahlPersonenInWarteschlange)
}
let anzahlPersonenInWarteschlange = 0
initialisiereLoRaVerbindung()
anzahlPersonenInWarteschlange = 0
sendeUndZeigePersonenanzahl()
```

## 👥 Senden der Daten in der Funktion "zeigePersonenanzahl" - Teil 1

ToDo.
* ToDo 

```blocks
function sendeUndZeigePersonenanzahl () {
    IoTCube.addUnsignedInteger(eIDs.ID_0, anzahlPersonenInWarteschlange)
    IoTCube.SendBufferSimple()
    basic.showNumber(anzahlPersonenInWarteschlange)
    warte_5_Sekunden_mit_Anzeige()
}
let anzahlPersonenInWarteschlange = 0
initialisiereLoRaVerbindung()
anzahlPersonenInWarteschlange = 0
sendeUndZeigePersonenanzahl()
```



## Gratuliere 🏆 - du hast das Tutorial erfolgreich bearbeitet 🚀

* Verbinde deine Warteschlangen mit dem Warteschlangen Widget der [Claviscloud](https://iot.claviscloud.ch/)! 
* Teste, ob die Daten korrekt angezeigt werden!

Behebe gegebenenfalls aufgetretene Fehler. Klicke auf das 💡- Symbol, um den gesamten Code der "Warteschlange" anzuzeigen.

```blocks
function initialisiereLoRaVerbindung () {
    smartfeldAktoren.oledInit(128, 64)
    smartfeldAktoren.oledClear()
    smartfeldAktoren.oledWriteStr("Verbinde")
    IoTCube.LoRa_Join(
    eBool.enable,
    eBool.enable,
    10,
    8
    )
    while (!(IoTCube.getStatus(eSTATUS_MASK.JOINED))) {
        basic.pause(1000)
        smartfeldAktoren.oledWriteStr(".")
    }
    smartfeldAktoren.oledClear()
    smartfeldAktoren.oledWriteStr("Verbunden!")
    basic.pause(2000)
    smartfeldAktoren.oledClear()
}
function sendeUndZeigePersonenanzahl () {
    IoTCube.addUnsignedInteger(eIDs.ID_0, anzahlPersonenInWarteschlange)
    IoTCube.SendBufferSimple()
    basic.showNumber(anzahlPersonenInWarteschlange)
    warte_5_Sekunden_mit_Anzeige()
}
function warte_5_Sekunden_mit_Anzeige () {
    smartfeldAktoren.oledClear()
    for (let fortschritt = 0; fortschritt <= 100; fortschritt++) {
        smartfeldAktoren.oledLoadingBar(fortschritt)
        basic.pause(50)
    }
    smartfeldAktoren.oledClear()
}
let anzahlPersonenInWarteschlange = 0
initialisiereLoRaVerbindung()
anzahlPersonenInWarteschlange = 0
sendeUndZeigePersonenanzahl()
basic.forever(function () {
    if (input.buttonIsPressed(Button.A)) {
        anzahlPersonenInWarteschlange += 1
        sendeUndZeigePersonenanzahl()
    }
    if (input.buttonIsPressed(Button.B)) {
        anzahlPersonenInWarteschlange += -1
        if (anzahlPersonenInWarteschlange < 0) {
            anzahlPersonenInWarteschlange = 0
        }
        sendeUndZeigePersonenanzahl()
    }
    basic.pause(100)
    basic.clearScreen()
})
```

```template
function zeigePersonenanzahl () {
    basic.showNumber(anzahlPersonenInWarteschlange)
}
let anzahlPersonenInWarteschlange = 0
anzahlPersonenInWarteschlange = 0
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