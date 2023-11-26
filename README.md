# neopixelplay
Playing with LED hardware friends!


## Setup Raspberry Pi (as usual)
- https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up

## Verify you can connect with Putty via SSH to that sweet raspberry pi
- __Instructions Placeholder__
- https://pimylifeup.com/raspberry-pi-enable-ssh-without-monitor/

## Setup Neopixels dependancies
When you've connected to your Pi, install the dependancies for the hardware
- `sudo pip3 install rpi_ws281x adafruit-circuitpython-neopixel`
- `sudo python3 -m pip install --force-reinstall adafruit-blinka`

## Setup Pi connections to hardware
- These are the strips I'm using - just make sure you have Addressable LEDs https://www.amazon.ca/gp/product/B01LSF4QDM/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1
- Note pinout - ground, power (5v) and GPIO pin 18 for data
<img width="1091" alt="image" src="https://github.com/kennyvigar/neopixelplay/assets/23610226/0d0bd6bd-70c6-45e5-8e47-dd77465921aa">

  - Note the arrow on your LED strips - this uh.. matters.  Don't ask ;)
    - (because you asked..) this is the data flow.  If you hook up backwards, nothing will work.
    - Arrow should point away from your Pi.
<img width="526" alt="image" src="https://github.com/kennyvigar/neopixelplay/assets/23610226/62fb0813-89c5-449c-9a35-0aeafb9858d5">
 
## Connect to Pi with VS Code
- Use the VS Code extension Remote-SSH from Micrsoft
<img width="833" alt="image" src="https://github.com/kennyvigar/neopixelplay/assets/23610226/289fec18-14fc-449d-a2b9-070de1310022">


## Sample Script for Testing
- sample here - bit of a tl;dr below, https://github.com/adafruit/Adafruit_CircuitPython_NeoPixel/blob/main/examples/neopixel_simpletest.py

```

import time
import board
import neopixel

#This is your Data Pin - we chose 18
pixel_pin = board.D18

# The number of NeoPixels
num_pixels = 260

# The order of the pixel colors - RGB or GRB. Some NeoPixels have red and green reversed!
# For RGBW NeoPixels, simply change the ORDER to RGBW or GRBW.
ORDER = neopixel.GRB

#Pixel Object
pixels = neopixel.NeoPixel(
    pixel_pin, num_pixels, brightness=0.2, auto_write=False, pixel_order=ORDER
)


def wheel(pos):
    # Input a value 0 to 255 to get a color value.
    # The colours are a transition r - g - b - back to r.
    if pos < 0 or pos > 255:
        r = g = b = 0
    elif pos < 85:
        r = int(pos * 3)
        g = int(255 - pos * 3)
        b = 0
    elif pos < 170:
        pos -= 85
        r = int(255 - pos * 3)
        g = 0
        b = int(pos * 3)
    else:
        pos -= 170
        r = 0
        g = int(pos * 3)
        b = int(255 - pos * 3)
    return (r, g, b) if ORDER in (neopixel.RGB, neopixel.GRB) else (r, g, b, 0)


def rainbow_cycle(wait):
    for j in range(255):
        for i in range(num_pixels):
            pixel_index = (i * 256 // num_pixels) + j
            pixels[i] = wheel(pixel_index & 255)
        pixels.show()
        time.sleep(wait)
while True:
     rainbow_cycle(0.001)  # rainbow cycle with 1ms delay per step

```


- Run with `sudo` on a Pi.
  `~/code/leds $ sudo python3 test.py`

- Lame picture - will upload a video when the kids are a little quieter in the background ;)
  <img width="1077" alt="image" src="https://github.com/kennyvigar/neopixelplay/assets/23610226/abf5abfd-b08a-4939-b8c4-35a0e887ddac">


## Sick reference here
- https://learn.adafruit.com/adafruit-neopixel-uberguide/python-circuitpython
  
