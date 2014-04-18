# pi-pins

Control, read, and monitor Linux GPIO devices from node.js.

A little set of interrupt-supporting wrappers around `"/sys/class/gpio"` interface, to control pins on e.g. the Raspberry Pi or Beaglebone, as originally written to help get [node-nrf](https://github.com/natevw/node-nrf) off the ground.

It's synchronous/blocking because I forget why (probably to match Tessel?)

## Example usage

First add a jumper wire from GPIO pin 17 to GPIO pin 22, and `npm install pi-pins`. Then:

    var pin1 = require("pi-pins").connect(17),
        pin2 = require("pi-pins").connect(22);
    pin2.mode('in');
    pin1.mode('high'); console.log("Should be true:", pin2.value());
    pin1.mode('low'); console.log("Should be false:", pin2.value());
    pin2.on('rise', function () {       // …or `'fall'`, or `'both'`
        console.log("RING A DING A LING");
    });
    pin2.value(true);


## API

Here are its methods:

* `var GPIO = require('pi-pins');` — import suggestion, used below
* `var pin = GPIO.connect(number)` — get pin ready for use. Could very possibly fail if you don't use `sudo` to run your script or set perms or whatever. but then, the software and hardware, working together, as one, until the very end
* `pin.mode(direction)` — set pin direction to `'in'` or `'out'` (or go straight to `'low'` or `'high'` output)
* `pin.value()` — read the pin's value (`'in'` mode)
* `pin.value(boolean)` — set the pin's value (`'out'` mode)


It also does [events](http://nodejs.org/api/events.html) (parties, weddings):

* `'rise'` — emitted when `gpio.value()` goes from `false` to `true`
* `'fall'` — emitted when `gpio.value()` goes from `true` to `false`
* `'both'` — emitted whenever `gpio.value()` changes

If a tree might fall/rise/both in the forest and nobody is watching, neither is `pi-pins`.


## Licentious

I sure hope not. But otherwise:

Copyright (c) 2013–2014, Nathan Vander Wilt

Permission to use, copy, modify, and/or distribute this software for any purpose with or without fee is hereby granted, provided that the above copyright notice and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.