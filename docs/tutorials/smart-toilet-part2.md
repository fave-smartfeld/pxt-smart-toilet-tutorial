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

* Ein LoRa- Gateway🛜 muss in Reichweite sein, welches mit TTN (The Things Network) verbunden ist.
Dies ist im Klassensatz einmal vorhanden und kann hunderte von IoT- Cubes bedienen.
![Bild](https://reifab.github.io/pxt-iot-tutorial/static/tutorials/gateway-klein.png)

## Lernergebnis

Aus dem Tutorial Teil 1 hast du bereits ein Programm, das den Status der Toilette "Besetzt" | "Frei" simuliert. 
Nun wollen wir über LoRa🛜 den Status ins Internet senden. Am Ende hast du ein funktionsfähiges Programm, das:

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
Nun bauen wir eine Verbindung zum Internet auf.
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


## 🛜 Status Verbunden anzeigen
Die Schleife wird beendet, wenn die Verbindung besteht, d.h. wir können ersetzen das Verbindungssymbol kurz durch ein bestätigendes Symbol ✔.

* Ziehe den Block ``||basic:zeige Symbol ✔ ||`` nach der **Während** Schleife und vor den Aufruf der Funktion **macheFrei**.
* Warte im Anschluss 1 Sekunde (1000 ms). Nutze ``||basic:pausiere (ms)||``.
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
basic.pause(1000)
macheFrei()
```

## Status "Frei" beim Start senden - notendige Variablen

Zu Beginn ist die Toilette immer frei, d.h. wir wollen nach dem Verbindungsaufbau diesen Status senden. Problematisch dabei ist, dass
nur alle 5 Sekunden eine Information an die Claviscloud geschickt werden kann. Deshalb müssen wir noch etwas abwarten bzw. den Zeitpunkt des letzten Sendevorganges uns merken und den Status **sendeErlaubnis** entsprechend anpassen.

* Um die Millisekunden seit dem letzten Senden zu wissen, benötigen wir eine Variable: ``||variables:Erstelle eine Variable...||``und benenne sie mit **msBeiLetztemSenden**.
* Die Millisekunden seit dem letzten Senden entsprechend den aktuellen Millisekunden. Setze deshalb nach dem erfolgreichen Verbinden bzw. dem Symbol ✔ die Variable auf ``||control:Millisekunden||`` 🕒.
* Auch für den Status, ob wir Daten senden dürfen, benötigen wir eine Variable: ``||variables:Erstelle eine Variable...||`` und benenne sie mit **sendeErlaubnis**.
* Da wir uns soeben erst verbunden haben, müssen wir gleichfalls die 5 Sekunden abwarten bzw. die die **SendeErlaubnis** auf falsch setzen: ``||variables:setze sendeErlaubnis ||`` auf ``||logic:false ||``.
* Auch für den Status, ob wir die Daten später senden sollen, benötigen wir eine Variable: ``||variables:Erstelle eine Variable...||`` und benenne sie mit **spaeterSenden**.
* Setze **SendeErlaubnis** gleichfalls auf falsch: ``||variables:setze spaeterSenden ||`` auf ``||logic:false ||``.

```blocks
let msBeiLetztemSenden = control.millis()
let sendeErlaubnis = false
let spaeterSenden = false
macheFrei()
```

## Funktion zum Senden der Daten erstellen - Teil 1

Am Beginn legen wir eine Funktion **sendeDaten** an, welche den zu sendenden Status als Parameter übergeben bekommt. Dazu:

* Hol dir den Block ``||functions:Erstelle eine Funktion...|`` und benenne die Funktion "sendeDaten". Zudem **Füge einen Parameter** mit dem Datentyp für eine **Zahl** mit dem Namen **status** hinzu. 
* Hol dir den Block ``||logic:Wenn dann ansonsten |`` und ziehe diesen in den Block der Funktion **sendeDaten**.
* Als Bedingung im Wenn-Block nutze einen Vergleich: Nutze in der Wenn-Bedingung einen Vergleich ``||logic:Vergleich für Dezimalzahlen |`` und ändere das Vergleichssymbol auf **>**. 
* Um zu prüfen, ob 5 Sekunden bereits abgelaufen sind, muss die aktuelle Zeit mit den **msBeiLetztemSenden** verglichen werden. Deshalb ziehe die aktuellen Millisekunden  ``||control:Millisekunden||`` 🕒 in den linken Bereich des Vergleichs.
* Im rechten Bereich des Vergleichs ergänze eine mathematische Berechnung. Denn die aktuelle Zeit muss um 5 Sekunden (5000 Millisekunden) grösser sein als die letzte Sendezeit. Ziehe den Block ``||math: + ||`` in den rechten Bereich des Vergleichs der Wenn-Bedingung.
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

* Hol dir den Block ``||IoTCube: Wahrheitswert mit der ID_0 ||`` und setze den Wert auf den **status** (= übergebener Parameter der Funktion). Dazu musst du den Paramter **status** anklicken und in den dafür vorgesehenen Bereich ziehen.
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
        sendeErlaubnis = false
        msBeiLetztemSenden = control.millis()
    } else {
    	
    }
}
```

## Funktion zum Senden der Daten erstellen - Teil 3

Jetzt legen wir fest, was passiert, wenn wir nicht senden dürfen:

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