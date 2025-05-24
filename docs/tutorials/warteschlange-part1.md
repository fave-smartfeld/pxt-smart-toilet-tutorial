```package
iot-cube=github:Smartfeld/pxt-iot-cube#v1.1.2
sensors=github:Smartfeld/pxt-sensorikAktorikSmartfeld
```
### @explicitHints false

# Warteschlangen Tutorial - Teil 1

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
die rote Markierung:
![Bild](https://reifab.github.io/pxt-iot-tutorial/static/tutorials/iot-cube-anschliessen-klein.png)
* Stelle die Schalter vorerst so ein:
    * Battery Switch: **off**
    * LoRa Module: **on**
![Bild](https://reifab.github.io/pxt-iot-tutorial/static/tutorials/iot-cube-power-switches-klein.png)
* Überprüfe, ob der micro:bit verbunden ist.

## Lernergebnis

In diesem Tutorial baust du Schritt für Schritt ein Programm auf, das eine Warteschlange simuliert. Am Ende hast du ein funktionsfähiges Programm, das...

* die Anzahl der Personen in der Warteschlange 👥 anzeigt.
* per ``||input:Knopfdruck||`` die Anzahl anpasst:
    * ``||Input:Knopf A ist geklickt||``: Eine Person stellt sich in die Warteschlange 👥.
    * ``||Input:Knopf B ist geklickt||``: Eine Person verlässt die Warteschlange 👤.

Wir starten langsam....

## 👥 Variable für die Anzahl an Personen in der Warteschlange

Um die Anzahl der Personen in der Warteschlange zu speichern, nutzen wir eine Variable **anzahlPersonenInWarteschlange** 👥.
* ``||variables:Erstelle eine Variable...||`` und benenne sie mit **anzahlPersonenInWarteschlange** 👥.
* Am Beginn nehmen wir an, dass keine Personen in der Warteschlange sind. Setze deshalb ``||basic:beim Start||``, die zuvor angelegte Variable ``||variables:setze anzahlPersonenInWarteschlange auf 0||``.

```blocks
let anzahlPersonenInWarteschlange = 0
```

## 👥 Anzeige der Anzahl der Personen auf der LED Matrix

Um die aktuelle Anzahl der Personen in der Warteschlange anzuzeigen, nutzen wir die LED Matrix.
* Hol dir den Block ``||basic:Zeige Zahl|`` und ziehe diesen in den Block **beim Start** direkt unter den Block ``||variables:setze anzahlPersonenInWarteschlange auf 0||`` 👥.
* Um den Wert der Variable **anzahlPersonenInWarteschlange** anzuzeigen, ziehe die ``||variables: anzahlPersonenInWarteschlange||`` anstatt 0 in den Block ``||basic:Zeige Zahl|``.
* 📥 Drücke `|Download|` und kontrolliere die LED-Anzeige:  
Wird die Zahl 0 dargestellt?

```blocks
let anzahlPersonenInWarteschlange = 0
anzahlPersonenInWarteschlange = 0
basic.showNumber(anzahlPersonenInWarteschlange)
```

## Funktion für das Anzeigen der Anzahl der Personen auf der LED Matrix

Da wir später bei Knopf B, nachdem wir die Anzahl reduziert haben, auch die Anzahl anzeigen möchten, erstellen wir eine Funktion **zeigePersonenanzahl** für diese Aufgabe.
* Hol dir den Block ``||functions:Erstelle eine Funktion...|`` und benenne die Funktion **zeigePersonenanzahl**.
* Nimm den Block ``||basic:Zeige Zahl|`` aus dem "beim Start" und ziehe diese in die soeben angelegte Funktion. 
* Hol dir den Block ``||functions:Aufruf zeigePersonenanzahl |`` und ziehe diesen in den Block **beim Start**.
* 📥 Drücke `|Download|` und kontrolliere die LED-Anzeige:  
Wird die Zahl 0 dargestellt?

```blocks
function zeigePersonenanzahl () {
    basic.showNumber(anzahlPersonenInWarteschlange)
}
let anzahlPersonenInWarteschlange = 0
zeigePersonenanzahl()
```

## 👥 Anzahl der Personen in der Warteschlange mit Knopf A erhöhen

Um die Anzahl an Personen in der Warteschlange zu erhöhen, nutzen wir eine logische Abfrage in der **dauerhaft**-Schleife:
* Hole dir den Block ``||logic:Wenn wahr dann||`` Block und ziehe diesen in die dauerhaft-Schleife.
* Ziehe den den Block ``||input:Knopf A ist geklickt||`` anstatt **wahr** in die ``||logic:Wenn wahr dann||`` .
* Um die Variable **anzahlPersonenInWarteschlange** zu erhöhen, nutze ``||variables:ändere anzahlPersonenInWarteschlange|`` um 1.
* Um den aktuellen Wert anzuzeigen, hole dir den Block ``||functions:Aufruf zeigePersonenanzahl|`` und ziehe diesen Block an das Ende in die **Wenn Kopf A ist geklickt dann**.
* 📥 Drücke `|Download|` und kontrolliere die LED-Anzeige:  
Wird die am Beginn die Zahl 0 dargestellt?<br />
Drücke Knopf A und beobachte was passiert?<br />
Wird dir 1, 2, 3, ... angezeigt bzw. die Anzahl der Personen in der Warteschlange erhöht?

```blocks
function zeigePersonenanzahl() {
    basic.showNumber(anzahlPersonenInWarteschlange)
}
let anzahlPersonenInWarteschlange = 0
zeigePersonenanzahl()
basic.forever(function () {
    if (input.buttonIsPressed(Button.A)) {
        anzahlPersonenInWarteschlange += 1
        zeigePersonenanzahl()
    }
})
```

## 👥 Anzahl der Personen in der Warteschlange mit Knopf B reduzieren

Um die Anzahl an Personen in der Warteschlange mit Knopf B zu reduzieren, nutzen wir wieder eine logische Abfrage in der **dauerhaft**-Schleife:
* Hole dir den Block ``||logic:Wenn wahr dann||`` und ziehe diesen nach die erste **Wenn Knopf A ist geklickt** in dauerhaft-Schleife.
* Ziehe den Block ``||input:Knopf A ist geklickt||`` anstatt **wahr** in die ``||logic:Wenn wahr dann||`` und ändere **A** auf **B** um.
* Um die Variable **anzahlPersonenInWarteschlange** zu reduzieren, nutze ``||variables:ändere anzahlPersonenInWarteschlange|`` um -1.
* Um den aktuellen Wert anzuzeigen, nutzen wir wieder den Block ``||functions:Aufruf zeigePersonenanzahl|``. Ziehe diesen Block an das Ende in die **Wenn Kopf B ist geklickt dann**.
* 📥 Drücke `|Download|` und kontrolliere die LED-Anzeige:  
Drücke Knopf B und beobachte was passiert?<br />
Wird dir -1, -2, -3, ... angezeigt bzw. die Anzahl der Personen in der Warteschlange reduziert?

**Eine negative Anzahl an Personen in einer Warteschlange macht keinen Sinn!**<br />
Wir müssen unser Programm noch erweitern...

```blocks
function zeigePersonenanzahl() {
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
        anzahlPersonenInWarteschlange += -1
        zeigePersonenanzahl()
    }
})
```

## 👥 Anzahl der Personen in der Warteschlange darf nicht negativ sein

Damit die Anzahl an Personen in der Warteschlange nicht negativ werden kann, setzen wir in derartigen Fällen die Variable **anzahlPersonenInWarteschlange** auf 0:
* Hole dir den Block ``||logic:Wenn wahr dann||`` und ziehe diesen nach den Block ``||variables:ändere anzahlPersonenInWarteschlange||`` um -1.
* Ziehe den Vergleich-Block ``||input:0 < 0||`` anstatt **wahr** in die ``||logic:Wenn wahr dann||``.
* Da wir wissen möchten, ob die Variable **anzahlPersonenInWarteschlange** negativ ist, ziehe die Variable ``||variables:anzahlPersonenInWarteschlange|`` anstatt den ersten **0** in den Vergleich. Dies prüft, ob der aktuelle Wert der Variable **anzahlPersonenInWarteschlange** kleiner als 0 ist.
* Wenn das der Fall ist, setzen wir die Variable **anzahlPersonenInWarteschlange** auf 0. Ziehe dazu den Block ``||variables:setze anzahlPersonenInWarteschlange|`` auf 0 in die **Wenn anzahlPersonenInWarteschlange < 0**.
* 📥 Drücke `|Download|` und kontrolliere die LED-Anzeige:  
Drücke Knopf B und prüfe, ob die Anzahl nicht mehr negativ wird?<br />
Prüfe, ob mit Knopf A die Anzahl erhöht und mit Knopf B die Anzahl reduziert wird...

```blocks
function zeigePersonenanzahl() {
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
        anzahlPersonenInWarteschlange += -1
        if (anzahlPersonenInWarteschlange < 0) {
            anzahlPersonenInWarteschlange = 0
        }
        zeigePersonenanzahl()
    }
})
```

## Gratuliere 🏆 - du hast den Teil 1 erfolgreich bearbeitet 🚀

Im nächsten Teil werden wir die Anzahl an Personen in der Warteschlange an die Claviscloud senden.

* Weiter gehts mit Teil 2: [Teil 2](https://makecode.microbit.org/#tutorial:github:fave-smartfeld/pxt-smart-toilet-tutorial/docs/tutorials/warteschlange-part2)
* Bitte überprüfe zuvor, ob dein Programm funktioniert!
  * Kannst du mit Kopf A die Anzahl an Personen erhöhen?<br />
  * Kannst du mit Kopf B die Anzahl an Personen reduzieren?<br />
  * Wird die Anzahl an Personen nie negativ auch wenn du häufiger Knopf B als Knopf A drückst?<br />
* Falls irgendwas noch nicht richtig läuft, hier hast Du eine funktionierende Version zum testen: [Lösung Teil 1](https://makecode.microbit.org/#tutorial:github:fave-smartfeld/pxt-smart-toilet-tutorial/docs/tutorials/warteschlange-part1-solution)