
# Particle Projects are now hosted on [github][1].

I have added my c++ code working working with the wifi capable microcontollers
from [particle.io](particle.io). Before working with the ocde in the repository
I was finding it very hard to work on multiple projects simutaneously. 

# Projects 

## Send restful commands to control RGB on device

A simple project that requires no additional hardware to run. The main.ino file
is included but really doesn't do anything but include the relevant files.

### Cmd module

The Cmd module was design to allow functions to be run by refereing to them by
their name or id. The module allows other libraries (or the user) to easily add
functions. This was done in cmd\_rgb.cpp.

### Demo via a bash script

The demo "demo\_cmd_rgb" script shows how easy it is to access any of these
functions. There is one script included to build the code and flash a device.
The second script will execute some of the commands that have been demonstrated.

### Code for this project

The full code source can be found on the new [github repository][1], but the
cmd module has been included in the notebook because it is the key component. The
rgb module can also been included to demonstrate how easy it is to add commands.

## Tasker (for Android) interacts with Particle's API

### Tasker allows the RGB to be controlled by an Android phone.

To demonstrate the accessibility of the capabilities exposed by the cloud device,
I have developed a Tasker project that allows the RGB commands to be executed.
Sensitive information has been removed. The variable ACCESS_TOKEN, and the various
device IDs would have to be set for someone else to utilize.

## Matlab function for interfacing with the particle api

I provide a function "Particle" which accepts a access_token that is tied to a
Particle.io account. The function returns a struct that contains each device
connected with the account. It will then determine both the functions and the
variables exposed for the device.

Makes it easy to call a function (do) on the device and pass the argument (led_on):
`P.Matt.call_do('led_on')`. It even exposes individual commands that are
contained in the device's variable named cmds. Then command can be excessed:
`P.Matt.do.led_on()`. Note that the list of cmds is listed in the *do* struct 
inside the device but could also be accessed via: `P.Matt.get_cmds()`.

- Given simply the access token, it will determine the devices available.
- Then it will determine the exposed functions and variables.
- It will also expose any commands listed in the cmds variable.

The function is the first matlab wrapper for the particle api that I have seen.

### Demo by controlling RGB

The bash script demonstrated earlier could easily be converted to a similar
matlab script. The functions can easily be exposed by exploting the anonymous
functions. A subset of the functionality has been demonstrated in a Matlab script.

[1]: https://github.com/NGenetzky/particle-projects