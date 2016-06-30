---
layout: single
title:  "CAN with Particle"
header:
  teaser: "unsplash-gallery-image-2-th.jpg"
  <!--teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"-->
tags:
  - tags
  - project_notebook
date:   2014-11-22
published: false
---

# CAN with Particle

## Research

### Datasheets

[Datasheet for the MCP2551][1]

### Background

Microchip : [A CAN Physical Layer Discussion][4]

### Examples

- I will refer to the [library made by Sparkfun][2].
- Another example ([Tiva C Series CAN Bus with MCP2551][3]).


## Getting Started

### Pinout on Particle
| Pin | Pin Function |
| --- | --- |
| Vin | USB V+ |
| GND | Ground |
| D2 | CAN2_RX |
| D1 | CAN2_TX |

<a id="Vin" /> When the Photon is powered via the USB port, VIN will output a voltage of approximately 4.8VDC due to a reverse polarity protection series schottky diode between V+ of USB and VIN.


### Pintout on MCP2551
| Pin Number | Pin Name | Pin Function |
| --- | --- | --- |
| 1 | TXD | Transmit Data Input |
| 2 | VSS | Ground |
| 3 | VDD | Supply Voltage |
| 4 | RXD | Receive Data Output |
| 5 | VREF | Reference Output Voltage |
| 6 | CANL | CAN Low-Level Voltage I/O |
| 7 | CANH | CAN High-Level Voltage I/O |
| 8 | RS | Slope-Control Input |

### Circuit
The Circuit is not overly complex, but will be discussed as two halves. The
circuit connected to Matt will be described, and Parker is the same circuit.
- The two share the same CANH and CANL nodes (these are not shown in circuit).
- They could also share the same 5V and GND.
- Termination resistors are between CANH and CANL on both sides, R1=R2=120 Ohms.
- Vin supplies 5V to MCP.
- The pins (D0, D1) on the Photons output 3.3V but are 5V tolerant.
- MCP will represent the MCP2551.

```json
{
    "nodes":["5V", "GND", "RX", "TX"],
    "connections":[
        "5V=Matt.Vin=MCP.VSS",
        "GND=Matt.GND=MCP.RS=MCP.VDD",
        "RX=Matt.D2=MCP.RXD",
        "TX=Matt.D1=MCP.TXD"
        ]
}
```

## Writing code for ASCI communication

I wrote functions that:
- Send a string of characters out on CAN as ASCI.
- Convert input data from CAN into ASCI and publish an event.

### What can the end user do?

- Send asci data in message with particlar CAN.id
- Observe the communication on CAN bus
    - Currently limited to 4 messages per second maximum. Works best for 1 message
per second.

### Working on Hex Communication

I am currently working on ways that the user can communicate with devices on the
CAN network by supplying data in the form of Hex. I am having to work with 
multiple conversion functions and the endiness of the data.

## CAN example for v1

The example will send a few strings between the devices.

# To be continued...

I am going to switch to working on other things but will return to CAN soon.


[1]: https://www.sparkfun.com/datasheets/DevTools/Arduino/MCP2551.pdf
[2]: https://github.com/sparkfun/SparkFun_CAN-Bus_Arduino_Library/
[3]: http://ohm.ninja/tiva-c-series-can-bus-with-mcp2551/
[4]: http://ww1.microchip.com/downloads/en/AppNotes/00228a.pdf