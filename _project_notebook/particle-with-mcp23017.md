
# Particle with MCP23017

## Reference

[16-Bit I/O Expander with Serial Interface][datasheet]

## Investigation

I used my previous i2c code to scan the i2c network to determine the address.
The IO Expander was at address 32 (0x20) when the hardware address was 0 
(aka A2=A1=A0=0V=GND).

## Don't Reinvent the Wheel

Instead of writing all the code from scratch I will be using existing libraries
to allow this device to be incorporated into my projects. Adafruit has a 
[library written for MCP23017][1], and there is a [version ported for Particle][2]. 

### Learn from library code

Quick easy bitwise read, set, clear, and writes. These *#define*s can be used
just like a C function (with uint8_t parameters). 
```Cpp
#define bitRead(value, bit) (((value) >> (bit)) & 0x01)
#define bitSet(value, bit) ((value) |= (1UL << (bit)))
#define bitClear(value, bit) ((value) &= ~(1UL << (bit)))
#define bitWrite(value, bit, bitvalue) (bitvalue ? bitSet(value, bit) : bitClear(value, bit))
```

## Projects that require a lot of pins.

### 4x4 Switch Matrix + 8 LED and 4 buttons

Parts:
    "4x5 Matrix Keyboard Buttons with Water Lights"

- The 4x4 button matrix requires 8 pins.
- The 5th row of buttons requires 4 pins.
- The 8 LEDs requires 8 pins.

The other 8 pins of the IO expander could be used all for the LEDs or 4 for the
LED and 4 for the buttons. I will likely only use four of the leds so that the
remaining LED could be used as status indicators.

#### Arduino-Keypad library

A library is available for interacting with a keypad. The digitalRead and 
digitalWrite functions used by the class could be overloaded because the library
designer was smart and made them a virtual function.
https://github.com/McTristan/Keypad-spark

### LED Matrixes

- 8x8 LED matrix requires 16 pins. 
- 8x8 Matrix RGB requires 32 pins!

### LCD through parallel interface

A 16x2 or 20x4 LCD could be attached to the IO expander and still have pins
avilable for other inputs. [DIY I2C LCD Display with Inputs][3] is an instructable
where this was done.


[1]: https://github.com/adafruit/Adafruit-MCP23017-Arduino-Library
[2]: https://github.com/pkourany/Adafruit_MCP23017_IDE
[3]: http://www.instructables.com/id/DIY-I2C-LCD-Display-With-Inputs/

[datasheet]: http://ww1.microchip.com/downloads/en/DeviceDoc/21952b.pdf
