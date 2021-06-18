# VS10xx Driver for MicroPython

This is an adoption of the great work for the VS1053 by [Uri Shaked](https://github.com/urish) for CircuitPython [here](https://github.com/urish/vs1053-circuitpython).

My board is a VS1003B and the code is working, therefore I'd rather call it a VS10xx library.

## Sample wiring

VS1003B Board | ESP32 PIN | Signal
--- | --- | ---
5V | 5V | VCC
GND | GND | Ground
XRST | GPIO21 | Reset (Active Low)
MISO | GPIO12 | Master In Slave Out
MOSI | GPIO13 | Master Out Slave In
SCLK | GPIO14 | SPI Clock Signal 
DREQ | GPIO22 | Data Request
XCS | GPIO25 | Send Command (Active Low)  
XDCS | GPIO23 | Send Data (Active Low)

## Sample code for the ESP32 board

```python
import vs10xx
from machine import SPI

spi = SPI(1, vs10xx.SPI_BAUDRATE) # SPI bus id=1 pinout: SCK = 14, MOSI = 13, MISO = 12

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
