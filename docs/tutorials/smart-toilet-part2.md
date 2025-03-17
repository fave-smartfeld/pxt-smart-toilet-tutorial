```package
iot-cube=github:Smartfeld/pxt-iot-cube#v1.1.2
sensors=github:Smartfeld/pxt-sensorikAktorikSmartfeld
```
### @explicitHints false

# Smart Toilet Tutorial - Teil 2

## 📗 Einführung,  Teil 2

**Vorraussetzungen**
🌱 IoT Basics abgeschlossen und Smart Toilet Tutorial [Teil 1 - noch ohne Internetverbindung](https://makecode.microbit.org/#tutorial:github:fave-smartfeld/pxt-smart-toilet-tutorial/docs/tutorials/smart-toilet-part1) abgeschlossen.

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

Aus dem Tutorial Teil 1 hast du bereits ein Programm, das den Status der Toilette "Besetzt" | "Frei" simuliert. 
Nun wollen wir den Status der Toilette "Besetzt" | "Frei" über LoRa🛜 den Status ins Internet senden. Am Ende hast du ein funktionsfähiges Programm, das...

* Eine LoRa-Verbindung🛜 aufbaut. 
* Den Status der Toilette 🚽 über LoRa🛜 sendet. 

Klicke auf das 💡- Symbol, um das vollständige Programm von Teil 1 anzuzeigen.

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


## 🛜 Verbindung mit Internet aufbauen
Am Beginne bauen wir eine Verbindung zum Internet auf.
Auf der LED-Matrix wollen wir den Verbindungsaufbau mit **🔱** anzeigen.

* Ziehe den Block 🛜``||IoTCube:LoRa Netzwerk-Verbindung||`` in 
``||basic:beim Start||`` vor den Funktionsaufruf **macheFrei** hinein.
* Ziehe darunter den Block ``||loops:während falsch mache||`` hinein. Weil
das Verbinden je nach Umständen 5 is 30 Sekunden dauert, wollen wir in dieser
Schleife verbleiben, solange die Verbindung noch **nicht** besteht.  
* Ziehe dazu den Block ``||Logic:nicht||`` auf die Schleife,
um den Wahrheitswert zu negieren.
* Füge in den **nicht** Block nun 🛜``||IoTCube:Lese Gerätestatus-Bit||`` ein.
Ändere darin das Bit auf "Verbunden". Der Code in der Schleife lautet nun "während nicht Lese gerätestatus-Bit verbunden". Programmierer/innen lesen den Code so: 
"Während das Gerät nicht verbunden ist." 
* Warte in der Schleife 1 Sekunde (1000 ms). Nutze ``||basic:pausiere (ms)||``.

```blocks
IoTCube.LoRa_Join(
eBool.enable,
eBool.enable,
10,
8
)
while (!(IoTCube.getStatus(eSTATUS_MASK.JOINED))) {
    basic.showLeds(`
        # . # . #
        # . # . #
        # # # # #
        . . # . .
        . . # . .
        `)
    basic.pause(1000)
}
macheFrei()
```


## 🛜 Status Verbunden ✔ anzeigen
Die Schleife wird beendet, wenn die Verbindung besteht, d.h. wir können ersetzen das Verbindungssymbol kurz durch ein bestätigendes Symbol ✔.

* Ziehe den Block ``||basic:zeige Symbol ✔ ||`` nach der **Während** Schleife und vor den Aufruf der Funktion **macheFrei**.
* Warte im Anschluss 5 Sekunden (5000 ms) ⏳. Nutze ``||basic:pausiere (ms)||``.
* Drücke 📥`|Download|` und kontrolliere die Anzeige auf der LED-Matrix.

Wird dir zuvor 🔱 und im Anschluss das Symbol ✔ angezeigt?

Wenn ja, dann ist dein IoT Cube mit dem Internet 😊 verbunden und kann Daten senden.

```blocks
IoTCube.LoRa_Join(
eBool.enable,
eBool.enable,
10,
8
)
while (!(IoTCube.getStatus(eSTATUS_MASK.JOINED))) {
    basic.showLeds(`
        # . # . #
        # . # . #
        # # # # #
        . . # . .
        . . # . .
        `)
    basic.pause(1000)
}
basic.showIcon(IconNames.Yes)
basic.pause(5000)
macheFrei()
```

## Status "Frei" beim Start senden - notwendige Variablen

Zu Beginn ist die Toilette immer frei, d.h. wir wollen nach dem Verbindungsaufbau diesen Status senden. Problematisch dabei ist, dass
nur alle 5 Sekunden eine Information an die Claviscloud geschickt werden kann. Deshalb müssen wir manchmal etwas abwarten ⏳ und uns den Zeitpunkt des letzten Sendevorganges merken und einen Status **spaeterSenden** ⏲️ nutzen.

* Um die Millisekunden seit dem letzten Senden zu wissen, benötigen wir eine Variable: ``||variables:Erstelle eine Variable...||``und benenne sie mit **msBeiLetztemSenden**.
* Die Millisekunden seit dem letzten Senden entsprechend den aktuellen Millisekunden. Setze deshalb nach dem erfolgreichen Verbinden bzw. dem Symbol ✔ die Variable auf ``||control:Millisekunden||`` 🕒.
* Gleichfalls benötigen wir für den Status, ob wir die Daten später senden müssen, eine Variable: ``||variables:Erstelle eine Variable...||`` und benenne sie mit **spaeterSenden**.
* Da wir uns soeben erst verbunden haben und die 5 Sekunden abgewartet haben, können wir die Variable **spaeterSenden** auf falsch setzen: ``||variables:setze sendeErlaubnis ||`` auf ``||logic:false ||``.

```blocks
let msBeiLetztemSenden = control.millis()
let spaeterSenden = false
macheFrei()
```

## Funktion zum Senden der Daten erstellen - Teil 1

Am Beginn legen wir eine Funktion **sendeDaten** an, welche den zu sendenden Status als Parameter übergeben bekommt. Dazu:

* Hol dir den Block ``||functions:Erstelle eine Funktion...|`` und benenne die Funktion "sendeDaten". Zudem **Füge einen Parameter** mit dem Datentyp für eine **Zahl** mit dem Namen **status** hinzu. 
* Hol dir den Block ``||logic:Wenn dann ansonsten |`` und ziehe diesen in den Block der Funktion **sendeDaten**.
* Als Bedingung im Wenn-Block nutze einen Vergleich: Nutze in der Wenn-Bedingung einen Vergleich ``||logic:Vergleich für Dezimalzahlen |`` und ändere das Vergleichssymbol auf **>**. 
* Um zu prüfen, ob 5 Sekunden bereits abgelaufen sind, muss die aktuelle Zeit mit den **msBeiLetztemSenden** verglichen werden. Deshalb ziehe die aktuellen Millisekunden  ``||control:Millisekunden||`` 🕒 in den linken Bereich des Vergleiches.
* Im rechten Bereich des Vergleiches ergänze eine mathematische Berechnung. Denn die aktuelle Zeit muss um 5 Sekunden (5000 Millisekunden) grösser sein als die letzte Sendezeit. Ziehe den Block ``||math: + ||`` in den rechten Bereich des Vergleiches der Wenn-Bedingung.
* Nutze die ``||variables:msBeiLetztemSenden |`` und addiere 5000 Millisekunden.

Klicke auf das 💡- Symbol, um zu überprüfen, ob du alle Schritte korrekt umgesetzt hast.

```blocks
function sendeDaten (status: number) {
    if (control.millis() > msBeiLetztemSenden + 5000) {

    } else {
    }
}
```


## Funktion zum Senden der Daten erstellen - Teil 2

Jetzt legen wir fest, was passiert, wenn wir senden dürfen:

* Hol dir den Block ``||IoTCube: Wahrheitswert mit der ID_0 ||`` und setze den Wert auf den **status** (= übergebener Parameter der Funktion). Dazu musst du den Parameter **status** anklicken und in den dafür vorgesehenen Bereich ziehen.
* Im Anschluss kannst du die Daten mit ``||IoTCube: Sende Daten ||``.
* Damit du erkennst, dass gesendet wurde, spiele ein ``||music: Mittleres C ||`` für einen Schlag bis zum Ende.
* Setze im Anschluss **spaeterSenden** auf falsch: ``||variables:setze spaeterSenden ||`` auf ``||logic:false ||``.
* Aktualisiere auch die **msBeiLetztemSenden** auf die aktuellen Millisekunden. Setze deshalb die Variable ``||variables:setze msBeiLetztemSenden ||`` auf ``||control:Millisekunden||`` 🕒.

Klicke auf das 💡- Symbol, um zu überprüfen, ob du alle Schritte korrekt umgesetzt hast.


```blocks
function sendeDaten (status: number) {
    if (control.millis() > msBeiLetztemSenden + 5000) {
        IoTCube.addBinary(eIDs.ID_0, status)
        IoTCube.SendBufferSimple()
        music.play(music.tonePlayable(262, music.beat(BeatFraction.Whole)), music.PlaybackMode.UntilDone)
        spaeterSenden = false
        msBeiLetztemSenden = control.millis()
    } else {

    }
}
```

## Funktion zum Senden der Daten erstellen - Teil 3

Jetzt legen wir fest, was passiert, wenn wir nicht unmittelbar senden können:

* Setze  **spaeterSenden** auf wahr: ``||variables:setze spaeterSenden ||`` auf ``||logic:true ||``.

Klicke auf das 💡- Symbol, um zu überprüfen, ob du alle Schritte korrekt umgesetzt hast.

```blocks
function sendeDaten (status: number) {
    if (control.millis() > msBeiLetztemSenden + 5000) {
        IoTCube.addBinary(eIDs.ID_0, status)
        IoTCube.SendBufferSimple()
        music.play(music.tonePlayable(262, music.beat(BeatFraction.Whole)), music.PlaybackMode.UntilDone)
        spaeterSenden = false
        msBeiLetztemSenden = control.millis()
    } else {
        spaeterSenden = true
    }
}
```

## Funktion zum Senden der Daten nutzen

Nachdem du die Funktion **sendeDaten** korrekt erstellt hast, nutzen wir diese zum Senden des aktuellen Status:

* Ergänze in der Funktion **macheFrei** nach der Anzeige der LED den Aufruf der Funktion: ``||functions: sendeDaten(statusFreiOderBesetzt) ||``.
* Ergänze in der Funktion **macheBesetzt** nach der Anzeige der LED den Aufruf der Funktion: ``||functions: sendeDaten(statusFreiOderBesetzt) ||``.
* Drücke 📥`|Download|` und drücke Knopf A, um zu simulieren, dass die Toilette besetzt ist.

Wird der Ton abgespielt? Wenn ja, dann werden die Daten an die Claviscloud gesendet? 
Überprüfe dies indem du prüfst, ob dein IoT Cube unter den Geräten aktiv ist!

Wenn nein, dann hast du nicht lange genug (5 Sekunden) gewartet. Diesen Fall lösen wir im nächsten Schritt. 

```blocks
function macheFrei () {
    statusFreiOderBesetzt = 1
    basic.showLeds(`
        . . # . .
        . # # # .
        # . # . #
        . . # . .
        . . # . .
        `)
    sendeDaten(statusFreiOderBesetzt)
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
    sendeDaten(statusFreiOderBesetzt)
}
function sendeDaten (status: number) {
    if (control.millis() > msBeiLetztemSenden + 5000) {
        IoTCube.addBinary(eIDs.ID_0, status)
        IoTCube.SendBufferSimple()
        music.play(music.tonePlayable(262, music.beat(BeatFraction.Whole)), music.PlaybackMode.UntilDone)
        spaeterSenden = false
        msBeiLetztemSenden = control.millis()
    } else {
        spaeterSenden = true
    }
}
```


## Dauerhaft prüfen, ob Daten zum Senden bereit liegen

Wir müssen noch den Fehler / Fall bearbeiten, wenn Daten nicht unmittelbar gesendet werden konnten. Dazu: 

* Hol dir den Block ``||loops: Alle 500ms ||`` und ziehe in einen freien Bereich.
* Hol dir den Block ``||logic:Wenn dann ||`` und ziehe diesen in den Block der soeben erstellten Schleife, welche alle 500 Millisekunden ausgeführt wird.
* Die Bedingung im Wenn-Block prüft, ob die Variable ``||variables: spaeterSenden ||`` wahr ist. 
* In diesem Fall muss die Funktion ``||functions: sendeDaten(statusFreiOderBesetzt) ||`` aufgerufen werden.
* Drücke 📥`|Download|` und drücke kurz nachdem das Symbol ✔ angezeigt wird den  Knopf A, um zu simulieren, dass die Toilette besetzt ist.

Wenn sich das Symbol gleich ändert, aber der Ton erst später abgespielt wird, dann hat alles funktioniert.

Wenn nicht, dann vergleiche deinen Code mit dem Code der Lösung, welche du [hier](https://makecode.microbit.org/_40wczFToxVE9) findest.

```blocks
loops.everyInterval(500, function () {
    if (spaeterSenden) {
        sendeDaten(statusFreiOderBesetzt)
    }
})
function sendeDaten (status: number) {
    if (control.millis() > msBeiLetztemSenden + 5000) {
        IoTCube.addBinary(eIDs.ID_0, status)
        IoTCube.SendBufferSimple()
        music.play(music.tonePlayable(262, music.beat(BeatFraction.Whole)), music.PlaybackMode.UntilDone)
        spaeterSenden = false
        msBeiLetztemSenden = control.millis()
    } else {
        spaeterSenden = true
    }
}
```


## Gratuliere 🏆 - du hast das Tutorial erfolgreich bearbeitet 🚀

* Verbinde deine Smart Toilet mit dem Toiletten Widget der [Claviscloud](https://iot.claviscloud.ch/)! 
* Teste, ob die Daten korrekt angezeigt werden!

Behebe gegebenenfalls aufgetretene Fehler. Klicke auf das 💡- Symbol, um den gesamten Code der "Smart Toilet" anzuzeigen.

```blocks
function macheFrei () {
    statusFreiOderBesetzt = 1
    sendeDaten(statusFreiOderBesetzt)
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
    sendeDaten(statusFreiOderBesetzt)
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
function sendeDaten (status: number) {
    if (control.millis() > msBeiLetztemSenden + 5000) {
        IoTCube.addBinary(eIDs.ID_0, status)
        IoTCube.SendBufferSimple()
        music.play(music.tonePlayable(262, music.beat(BeatFraction.Whole)), music.PlaybackMode.UntilDone)
        spaeterSenden = false
        msBeiLetztemSenden = control.millis()
    } else {
        spaeterSenden = true
    }
}
input.onButtonPressed(Button.B, function () {
    macheFrei()
})
let statusFreiOderBesetzt = 0
let spaeterSenden = false
let msBeiLetztemSenden = 0
IoTCube.LoRa_Join(
eBool.enable,
eBool.enable,
10,
8
)
while (!(IoTCube.getStatus(eSTATUS_MASK.JOINED))) {
    basic.showLeds(`
        # . # . #
        # . # . #
        # # # # #
        . . # . .
        . . # . .
        `)
    basic.pause(1000)
}
basic.showIcon(IconNames.Yes)
basic.pause(5000)
msBeiLetztemSenden = control.millis()
spaeterSenden = false
macheFrei()
loops.everyInterval(500, function () {
    if (spaeterSenden) {
        sendeDaten(statusFreiOderBesetzt)
    }
})
```