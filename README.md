# VS10xx Driver for MicroPython

This is an adoption of the great work by [Uri Shaked](https://github.com/urish) for CircuitPython [here](https://github.com/urish/vs1053-circuitpython).

My board is a VS1003B and the code is working, therefore I'd rather call it a VS10xx library.

## Sample wiring

VS1003B Board | ESP32 PIN 
--- | --- 
5V | 5V
GND | GND
XRST | 21
MISO | 12
MOSI | 13
SCLK | 14
DREQ | 22
XCS | 25
XDCS | 23

## Sample code for the ESP32 board

```python
import vs10xx
import board
from machine import SPI

spi = SPI(1, SPI_BAUDRATE) # SPI bus id=1 pinout: SCK = 14, MOSI = 13, MISO = 12

player = vs10xx.Player(
    spi,
    xResetPin = 21,
    dReqPin = 22,
    xDCSPin = 23,
    xCSPin = 25,
    CSPin = None
)

# copied a test.mp3 directly to the esp32 filesystem via WebREPL
inputFile = open('test.mp3', mode='rb')

buf = bytearray(32)
while inputFile.readinto(buf):
    player.writeData(buf)
```
