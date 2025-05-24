```package
iot-cube=github:Smartfeld/pxt-iot-cube#v1.1.2
sensors=github:Smartfeld/pxt-sensorikAktorikSmartfeld
```
### @explicitHints false

# Warteschlange Teil 1
## Lösung

* Unten die Lösung von Tutorial Teil 1 
* Drücke 📥`|Download|` und teste das Programm.

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
        anzahlPersonenInWarteschlange += -1
        if (anzahlPersonenInWarteschlange < 0) {
            anzahlPersonenInWarteschlange = 0
        }
        zeigePersonenanzahl()
    }
})
```