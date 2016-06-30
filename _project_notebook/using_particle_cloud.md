
# Notes about [particle-projects][1]

## Commands (cmd) (Particle.Function:do)

The cmds module made it easy to call commands (procedures) that performed an action.

### Example

Calling the function **do** with the value **"led_on"** will cause the board's led
to turn on.

### A list of all commands available (Particle.Variable:cmds)
The variable "cmds" has been very easy to work with in other applications. The 
ability to get names for every **function** that the microcontroller can do was
very usable for menus. 

## Variables (node)

### Set variable with put (Particle.Function:put)
The **put** function has been very useful to pass any object that can be translated
into a asci encoded string. The idea is that the user can provide a **key** that
should match to a variable name. They also provide a **value** that is asci encoded.

#### Put Example

A **Particle.Event** with the name **"put"** with the data **"put_rgb=255,255,255"** 
would create an event named **"put_rgb"** with the value **"255,255,255"** this would
then caused the rgb to change to the R,G,B code 255,255,255 (aka white).

### Get variables with do (Particle.Function:do)
If the rule could be enforced that for every **put** function there must also be
a **do** function that can be used to read the data. It may also perform actions
at that time. Before writing any data in this variable with **put** the variable's
**do** function could provide *help information* and *default values*.

#### Do Example
A **Particle.Event** with the name **"do"** with the data **"put_rgb"**  should 
reply with the value expected to be in **var** **"put_rgb"**.


## Streams (post)

Note: This section has not been developed yet and is this in the idea stages.


The **post** function is best used for supplying a string of data to a modules
subsystem. The **post** function could work good with modules like:
- Serial
- I2C
- Can
- Udp
- Tcp
- Spi
- SoftwareSerial

### Example

A **Particle.Event** with the name **"/i2c0/6"** with the data **"Hello"** would
then send the string "Hello" to the device at i2c address 6 (7bit).



[1]: https://github.com/NGenetzky/particle-projects
