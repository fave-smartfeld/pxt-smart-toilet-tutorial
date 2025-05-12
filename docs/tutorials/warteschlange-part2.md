```package
iot-cube=github:Smartfeld/pxt-iot-cube#v1.1.2
sensors=github:Smartfeld/pxt-sensorikAktorikSmartfeld
```
### @explicitHints false

# Warteschlangen Tutorial - Teil 2

## 📗 Einführung,  Teil 2

**Vorraussetzungen**

🌱 IoT Basics abgeschlossen und Warteschlangen Tutorial [Teil 1 - ohne Internetverbindung](https://makecode.microbit.org/#tutorial:github:fave-smartfeld/pxt-smart-toilet-tutorial/docs/tutorials/warteschlange-part1) abgeschlossen.

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

Aus dem Tutorial Teil 1 hast du bereits ein Programm erstellt, welches die Anzahl an Personen einer Warteschlange anzeigt. Die Anzahl kann mit Kopf A erhöht und B reduziert werden. 
Nun wollen wir die Anzahl an Personen in der Warteschlange über LoRa🛜 ins Internet senden. Am Ende hast du ein funktionsfähiges Programm, das...

* Eine LoRa-Verbindung🛜 aufbaut. 
* Die Anzahl an Personen in der Warteschlange 👥 über LoRa🛜 sendet. 

Das vollständige Programm aus Teil 1 ist bereits integriert. Falls dir etwas unklar ist, überlege nochmals den [Teil 1 des Tutorials](https://makecode.microbit.org/#tutorial:github:fave-smartfeld/pxt-smart-toilet-tutorial/docs/tutorials/warteschlange-part1) zu bearbeiten.

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

Jetzt möchten wir den erfolgreichen Verbindungsaufbaus kurz (2 Sekunden) auf dem OLED anzeigen.
* Ziehe dazu den Block ``||smartfeldAktoren:Display:Lösche Displayinhalt||`` ans Ende der Funktion bzw. nachdem die Schleife beendet ist, um den Inhalt des Displays zu löschen.
* Im Anschluss ziehe den Block ``||smartfeldAktoren:Display:schreibe String||`` **"Verbunden!"**, um zu zeigen, dass sich der IoT Cube erfolgreich verbunden hat.
* Um Strom zu sparen, löschen wir nach 2 Sekunden das Display wieder. Nutze dazu den Block ``||basic:pausiere (ms)||`` **2000ms** und ``||smartfeldAktoren:Display:Lösche Displayinhalt||``.
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

## 🛜 Senden der 👥 Personenanzahl in der Funktion "zeigePersonenanzahl"

Nach erfolgreichem Verbindungsaufbau können wir die aktuelle Personenanzahl an die Claviscloud senden. Dazu erweitern wir die Funktion "zeigePersonenanzahl".
* Benenne die Funktion **zeigePersonenanzahl** auf **sendeUndZeigePersonenanzahl** um. Dazu klickst du in den Namen der Funktion und änderst diesen entsprechend ab.
* Ziehe den Block ``||IoTCube: Ganzzahl mit ID_0 ||`` an den Beginn der Funktion **sendeUndZeigePersonenanzahl**.
* Ersetze die 0 mit dem aktuellen Variablenwert von **anzahlPersonenInWarteschlange** indem du den Block ``||variables:anzahlPersonenInWarteschlange ||`` in diesen Bereich ziehst.
* Im Anschluss kannst du mit ``||IoTCube: Sende Daten ||`` die Daten, welche in der *Ganzzahl mit ID_0* hinterlegt sind, an die Claviscloud senden.
* Da die Übermittlung Zeit benötigt und *nur* alle 5 Sekunden ein Sendevorgang stattfinden kann, integrieren wir noch eine Fortschrittsanzeige. 


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

## 🛜 Sendevorgang: Fortschrittsanzeige - Teil 1

Über LoRa kann *nur* alle 5 Sekunden ein Sendevorgang stattfinden. Deshalb integrieren wir eine Fortschrittsanzeige:
* Hol dir den Block ``||functions:Erstelle eine Funktion...||`` und benenne die Funktion "warte5SekundenUndZeigeFortschritt".
* Rufe diese Funktion am Ende der Funktion **sendeUndZeigePersonenanzahl** auf, d.h. integriere den Block ``||functions: Aufruf warte5SekundenUndZeigeFortschritt ||`` als letzten Schritt in die Funktion **sendeUndZeigePersonenanzahl**.
* Am Beginn der Funktion **warte5SekundenUndZeigeFortschritt** wollen wir das OLED-Display leeren. Hol dir dazu den Block ``||smartfeldAktoren:Display:Lösche Displayinhalt||`` und ziehe in in die Funktion ``||functions: warte5SekundenUndZeigeFortschritt ||``.
* Die Fortschrittsanzeige folgt im nächsten Schritt.

```blocks
function warte5SekundenUndZeigeFortschritt () {
    smartfeldAktoren.oledClear()
}
function sendeUndZeigePersonenanzahl () {
    IoTCube.addUnsignedInteger(eIDs.ID_0, anzahlPersonenInWarteschlange)
    IoTCube.SendBufferSimple()
    basic.showNumber(anzahlPersonenInWarteschlange)
    warte5SekundenUndZeigeFortschritt()
}
let anzahlPersonenInWarteschlange = 0
initialisiereLoRaVerbindung()
anzahlPersonenInWarteschlange = 0
sendeUndZeigePersonenanzahl()
```

## 🛜 Sendevorgang: Fortschrittsanzeige - Teil 2

Für die Fortschrittsanzeige nutzen wir einen Fortschrittsbalken des OLED-Displays:
* Hol dir den Block ``||loops:für index von 0 bis 4||`` und nennen die Variable *index* über das Auswahlmenü auf **fortschritt** um. Ändere die *bis 4* auf **bis 100** (Prozentueller Fortschritt) um.
* In der Schleife ergänze den Block ``||smartfeldAktoren:Display:zeichne Ladebalken bei 0 Prozent ||`` und nutze die Variable ``||variables:fortschritt ||`` anstatt der 0 für den Ladebalken.
* Zudem müssen wir mit ``||basic:pausiere 50(ms) ||``  Millisekunden warten, damit insgesamt 5 Sekunden (100 x 50 Millisekunden = 5000 Millisekunden = 5 Sekunden) verstreichen.
* Am Ende löschen wir das OLED-Display wieder. Hol dir dazu den Block ``||smartfeldAktoren:Display:Lösche Displayinhalt||`` und ziehe diesen Bock an das Ende der Funktion ``||functions: warte5SekundenUndZeigeFortschritt ||``.
* 📥 Drücke `|Download|` und kontrolliere das OLED-Display:  
Wird die nach Änderung der Personenanzahl die Fortschrittsanzeige angezeigt? <br />
Wird im Anschluss die Anzeige wieder gelöscht?

```blocks
function warte5SekundenUndZeigeFortschritt () {
    smartfeldAktoren.oledClear()
    for (let fortschritt = 0; fortschritt <= 100; fortschritt++) {
        smartfeldAktoren.oledLoadingBar(fortschritt)
        basic.pause(50)
    }
    smartfeldAktoren.oledClear()
}
function sendeUndZeigePersonenanzahl () {
    IoTCube.addUnsignedInteger(eIDs.ID_0, anzahlPersonenInWarteschlange)
    IoTCube.SendBufferSimple()
    basic.showNumber(anzahlPersonenInWarteschlange)
    warte5SekundenUndZeigeFortschritt()
}
let anzahlPersonenInWarteschlange = 0
initialisiereLoRaVerbindung()
anzahlPersonenInWarteschlange = 0
sendeUndZeigePersonenanzahl()
```

## Gratuliere 🏆 - du hast das Tutorial erfolgreich bearbeitet 🚀

* Verbinde deinen IoT Cube mit dem Warteschlangenprogramm mit dem Warteschlangen Widget der [Claviscloud](https://iot.claviscloud.ch/)! 
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
    warte5SekundenUndZeigeFortschritt()
}
function warte5SekundenUndZeigeFortschritt () {
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
        anzahlPersonenInWarteschlange += -1
        if (anzahlPersonenInWarteschlange < 0) {
            anzahlPersonenInWarteschlange = 0
        }
        zeigePersonenanzahl()
    }
})
```
