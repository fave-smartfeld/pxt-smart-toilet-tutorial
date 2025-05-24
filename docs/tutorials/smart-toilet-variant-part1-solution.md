```package
iot-cube=github:Smartfeld/pxt-iot-cube#v1.1.2
sensors=github:Smartfeld/pxt-sensorikAktorikSmartfeld
```
### @explicitHints false

# Smart Toilet Variante Teil 1
## Lösung

* Unten die Lösung von Tutorial Teil 1 
* Drücke 📥`|Download|` und teste das Programm.

```template
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
let statusFreiOderBesetzt = 0
macheFrei()
basic.forever(function () {
    if (input.buttonIsPressed(Button.A)) {
        macheBesetzt()
    }
    if (input.buttonIsPressed(Button.B)) {
        macheFrei()
    }
})
```