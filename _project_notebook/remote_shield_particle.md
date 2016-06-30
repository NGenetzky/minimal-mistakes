
# 2016-06-27 Working on Smart Car Remote

The objective of this session will be to demonstrate the capability that I have
implemented for (do/cmd) and (put), but also to explore the new remote I purchased.
Remote Shield was purchased from Ethan Pan at [freenove](http://www.freenove.com).

## Prefix: Lab notes are not like essays.

I am going to attempt to start creating lab notes that document my work as I work
on something rather than after. I will not get into details but basicly I think
I will close all lab notes at the end of the day and continue it on another day
if needed with a new note.

## Circuit
### Adapting Arduino shield to Particle Device
'A' will denote the Arduino mapping. 'P' will denote the Particle mapping. The
arduino mappings describe where the pin is on the shield (no arduino is involved).
```
P.Vin=A.5V
P.GND=A.GND
P.A0=A.A0
P.A1=A.A1
P.A2=A.A2
P.A3=A.A3

P.D2=A.D2
P.D3=A.D3
P.D4=A.D4
P.D5=A.D8

P.A4=A.D5
P.A5=A.D6
```

### Circuit Representation of the shield.

TODO: Draw a circuit that shows the switches, LEDs, potentiometers and joystick
as simple circuit components.

## Plan

The first program will regularily be reading the analog values into a structure.
When any button is pressed then an interrupt should be raised. It should be easy
to specify what happens on button press. Functions should be provided to control
the LEDs on the remote.

## Concerns

### Button Debounce

The switches must be debounced.

### Hiding Data Structures

The user of the module should not know how the data is stored. Therefore the
structures will be defined in the cpp file. The primary way the data is accessed
via the string representation.

## Features

### Analog values published to Thingspeak

Will be able to publish all the analog vales to a thingspeak channel. Requires
a function and a webhook.

### Button press duration

Instead of storing a boolean value for each button I will store a value that
indicates how long the button was pressed.


### Dynamically assign a command to a duration of button press.

Could conditionally perform different commands based on the duration of time the
button was depressed.

Currently it is easy to staticly choose different actions based on the interval.
```cpp
if( d_in.sw1 ){
        if( SW_INTERVAL * 10 < d_in.sw1 ){
            led3_on();
        } else if( SW_INTERVAL * 4 < d_in.sw1 ){
            led2_on();
        } else if( SW_INTERVAL * 1 < d_in.sw1 ){
            led1_on();
        } else {
            Particle.publish("scr::sw1", String(d_in.sw1), 60, PRIVATE);
        }
        
        d_in.sw1 = 0; // Zero the duration.
    }
```

Option 1 allows any count (of base interval):
```cpp
struct SwitchEvents {
    void sw_id;
    void count;
    void (*command) ( void );
};
```

Option 2 allows named durations:
```cpp
// WARNING: psuedo code.
Enum class SwitchDuration { SHORT, MEDIUM, LONG
struct SwitchEvents {
    void sw_id;
    SwitchDuration duration;
    void (*command) ( void );
};
```

## How I used the Particle cloud functionality

### "sc_remote", a Particle Variable

```cpp
// In setup
Particle.variable("sc_remote", data_string);
...
// Regularily calling:
void update(){
    sprintf(data_string, "(%u,%u),(%u,%u),(%u,%u,%u)",
            a_in.pot1, a_in.pot2, a_in.joyx, a_in.joyy,
            d_in.sw1, d_in.sw2, d_in.sw3);
}
```

### "thingspeak", a Paricle Event connected to webhook

Publishing a json object of analog values as an event allows the data to be 
sent to the ThingSpeak servers.

TODO: Implement without String
```cpp
void thingspeak_analog(){
    String json = String("{\"1\":\""
        + String(a_in.pot1)
        + "\",\"2\":\""
        + String(a_in.pot2)
        + "\",\"3\":\""
        + String(a_in.joyx)
        + "\",\"4\":\""
        + String(a_in.joyy)
        + "\"}");
    Particle.publish("thingspeak", json, 60, PRIVATE);
}
```

This is not code; it is a json object that is read by the Particle command line
to create a webhook.
```json
{
    "event": "thingspeak",
    "url": "https://api.thingspeak.com/update",
    "requestType": "POST",
    "form": {
        "api_key": "****************",
        "field1": "{{1}}",
        "field2": "{{2}}",
        "field3": "{{3}}",
        "field4": "{{4}}",
        "field5": "{{5}}",
        "field6": "{{6}}",
        "field7": "{{7}}",
        "field8": "{{8}}",
        "lat": "{{a}}",
        "long": "{{o}}",
        "elevation": "{{e}}",
        "status": "{{s}}"
    },
    "mydevices": true,
    "noDefaults": true
}
```


