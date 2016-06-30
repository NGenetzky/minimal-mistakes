
# Intro to Particle

## Devices / Products they have

### Wifi Enabled (Core and Photon)

I started with a single core, Comrad; however, now have 3 other photons: Matt,
Parker, Logan. These devices can connect to the cloud through any wifi access
point that can be accessed using a SSID and passphrase. If they have access to
a network they can also communicate using UDP or TCP communication over the
wifi network without connecting the Particle cloud.

### Cellular engabled (Electron)

I have one Electron, Eli. He is not restricted to only areas I have access to
wifi. I think this product has applications in mobile applications. I have
purchased a GPS device (serial communication) that I believe will work well
with the Electron.

## Firmware exposed through Cloud API

### Variables

A variable that is defined in the firmware can be made publiclly available.
The type can be int, double, or string; the string is far by the most capable.

### Function

A function that takes a string and returns an integer can be made publically
available. (Note that the argument type is Arduino's String type).

### Subscribe

A device can register for any events that start with certain characters. When
an event is recieved, the topic and the data are sent to a function. Particle
broadcasts standard events involving your devices.

By using **webhook** responses the device can also be provided with information
requested from the internet, but this is a more advanced use.

### Publish

A device can broadcast an event with a topic and data. Using **webhooks** or
**IFTTT** allows actions to be triggered when certain events are broadcasted. 

### Webhooks

Webhhoks are a little complicated and will be comered more throughly in the
future.

## RESTful API

The Particle team provides a command line interface (using Node.js) and a
javascript library.

I have interfaced with the api using Matlab, Python, and Tasker. I used an
unofficial library for python. Using the RESTful API with matlab was very
easy; however using it from tasker with more complicated. This is because
Tasker has a unique set of programming tools that is largely graphical.

### Call

The functions exposed on the device can be called through a RESTful API. This
is the easiest way to involve behavior on a specific device. 

### Get

The variables exposed on the device are accessible through the api. This is
the most reliable way of recieving data from the device. A large string of
characters can be exposed as a variable.

### Publish

A event can be published through the restful api. Mutiple devices can (and
typically do) subscribe to the same event topics. Events can be public or
privately published.

### Subscribe

The command line interface can be used to monitor events that meet specified
conditions. Accessing the the event stream published by a device is easy through
the **dashboard** on the particle.io but not very easy to access
programmatically. I am unable to subscribe to the "server-side-events" aka
"sse" from Matlab or Tasker.

## Network Communication

### UDP or TCP

There is support for firmware to communicate using UDP or TCP protocols. The
documentation on docs.particle.io provide a complete discription of the
functionality. For low latency or high bandwidth applications this
communication could prove valuable but it is more complicated that using the
cloud api.

### Cellular

There is support for the communication protocol used by phones on the
Electron, but I am not familiar with them yet.
