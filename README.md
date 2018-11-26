# rgb_ring_clock

Use Gumstix RoomSense and Adafruit NeoPixel Ring to create a sync'ed "Analog" clock.  There are two interesting components to this
sketch.
  First it uses NTP to keep accurate time, checking every 5 minutes or so.  This means that the '1-second tick' timer interrupt, which 
wasn't matching up perfectly for some reason, didn't have to be fine-tuned past +/- 0.01s.
  Secondly, I'm using a 16-segment LED ring to keep time for 1/60 and 1/12 increments.  In order to emulate a semi-analog motion with the
ring, the intensity of the 'hand's' next LED and the current LED to indicate that the time rests between the two.  It's still difficult to
read time accurately from the face, but it's a neat effect.


## Setup
I used a Gumstix RoomSense with the AutoBSP package for this project.  You can download and install 
this package following these directions:

### BSP

1. copy the following address to `file/preferences/settings/Additional Boards Manager URLs`:

https://dev-geppetto-data.s3.amazonaws.com/designs/bsp/08a989c3b3bb65e114375622a1a79f5e4c54a7b2/package_roomsense-w-gpio_0_index.json

2. go to `tools>board>Boards Manager...`

3. Set the `Type` drop-down box to `Contributed` to filter out most packages

4. Select and install `roomsense-w-gpio by Gumstix, Inc.`

5. Close boards manager and check that this is the board selected under `tools>board`


### Libraries

This sketch uses the following libraries:

* Adafruit ZeroTimer
* WiFi101 and WiFiUdp
* Adafruit NeoPixel

If you haven't installed these libraries, make sure you install them through the library 
manager (`sketch>include library>manage libraries...`)

### Hardware

For this sketch, I have the NeoPixel ring wired directly to the RoomSense.  I have not checked how many rings could be
strung together directly from the USB power, but it's probably not much more than two.  Adafruit recommends connecting
the rings' V+ and GND [directly to a 5V wall wart](https://learn.adafruit.com/adafruit-neopixel-uberguide/powering-neopixels)
to provide a suitable amount of power for maximum brightness.

I have hard-wired my ring directly to the unpopulated headers of the RoomSense.  On the left-hand header, solder the '5V DC' 
pin from the ring to the bottom pin (Labeled 5V).  You'll find GND on the 4th pin up from the bottom on the same header.

The data-in pin for the ring has been soldered to pin 7.  This corresponds to the PA21 pad on the SAMD21.  

AutoBSP packages define all pins used in a design by their pad IDs as well as by an index number.  Because it is programatically 
generated, the BSP's index number does not always align with the silkscreen on the design.


### WiFi

For reasons of "Don't publish your WiFi Credentials" the required header file for WiFi connectivity, `wifi_wpa.h` is in the
`.gitignore` file.  Instead there is a `wifi_wpa.h.dist` which should be renamed and your local SSID and PSK should replace the
default text in the file.

If connection fails the built-in LED will blink at you.

### Public Domain

I am providing this sketch in good faith.  Make use of it as you will, although if you use any of it in your own project, I'd love
a mention.
