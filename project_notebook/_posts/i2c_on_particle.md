---
layout: single
title:  "I2C on Particle"
header:
  teaser: "unsplash-gallery-image-2-th.jpg"
  <!--teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"-->
tags:
  - tags
  - project_notebook
date:   2014-11-22
published: false
---

# I2C on Particle

## Existing I2C code

### <a name="A1"/> 1st Attempt [**A1**](#f1)
My first attempt was all contained in a single **"main.ino"**. While this worked
it was very ugly and did not encapsulate the i2c functions.

#### main.ino
Uses an array for data recieved from i2c and for data meant to be sent out on
i2c. The board led will flash at a regular interval so that the user knows the
program is running.

#### example.bash
Will have Matt send a string to Parker over i2c. Then will request data from
Parker.

#### Exposed API
```
  Variables:
    bytes_on_req (int32)
    i2c_address (int32)
    i2c_target (int32)
    i2c_data (string)
    i2c_slaves (string)
  Functions:
    int i2c(String args) 
    int send(String args) 
    int target(String args)
```

#### Circuit
For now I won't go into much detail. There are three nodes . Below describes what
is attached to each. I used R1=R2=2.2 kOhms.

```
{
    "nodes":["3V3", "I2C_DATA", "I2C_CLK"],
    "connections":[
        "3V3=Parker.3V3=R1+=R2+",
        "I2C_DATA=Parker.D0=Matt.D0=R1-",
        "I2C_CLK=Parker.D1=Matt.D1=R2-"
        ]
}
```

### <a name="A2"/> 2nd Attempt [**A2**](#f2)
Next I split the major functions into **i2c\_particle.h** & **i2c\_particle.cpp** files.

#### main.ino
The *loop* in this file would execute a function based on a variable **sys**.
The function would then a segment of code based on another variable **state**.
The idea was to provide a way to implement separate [FSM][1] as each of the
functions and then provide a method to switch the system. This method
worked to implement multiple systems in one code base; however, it was overly
complex.

#### i2c\_particle (.h/.cpp)
This implementation relied on a ringbuffer that was defined as static voltile
within the cpp file. Any data recieved was send to the buffer. There were functions
to allow the buffer to be **published** or to be send out via i2c.

#### example.bash
I wrote a simple bash script that that demonstrated the functionality. *Matt* will
send a message to *Parker*, which will then publish that message.

#### Circuit
Same circuit as was used for [A1](#A1).

#### Exposed API
```
  Variables:
    state (int32)
    sys (int32)
    ms_per_loop (int32)
    wreg (string)
    my_address (int32)
    target (int32)
  Functions:
    int set_wreg(String args) 
    int act(String args) 
    int i2c(String args) 
```
#### Errors / TODO

Currently there is an error created in the example that I can not explain. I do
not intend to fix it in this code, but rather avoid it in the new revision.

## New I2C code

I am currently working on a new set of files to work with i2c. They are part of
a larger project. The description of the project will take some time and therefore
will be left for later. 

## Learned
1. I learned an easy way check if one string (cmd) starts with another ("target").

```c++
// Will check if cmd starts with "target"
if (strncmp(cmd, "target", strlen("target"))==0) {}
```

2. How to echo commands executed in Bash.

```bash
# Shows the commands as they are executed. ">> " appears before command.
trap 'echo ">> $BASH_COMMAND"' DEBUG
```

## Projects
<b id="f1">A1</b> [↩](#A1)
```json
{  
    "particle-i2c-a1":{  
        "files":{  
            "main.ino":"0a0ddb46816bb6825c63b632932301c5613969d2",
            "example.bash":"4a4abba0db9defc6bd855ebec3fc1a0947604a4e"
        }
    }
}
```
<b id="f2">A2</b> [↩](#A2)
```json
{
    "particle-i2c-a2": {
        "files": {
            "main.ino": "f94973feeb1839e5d76d446d0ee342ab7aeb77b7",
            "i2c_particle.h": "aabc99a75db76503237eb381260fc53fba2e8cec",
            "i2c_particle.cpp": "33868f19a51faa0720c330e3974ba3f22d1b4b63",
            "example.bash": "848035eafbdaade19491821fba1d42b7f363fd84"
        }
    }
}
```

[1]: https://en.wikipedia.org/wiki/Finite-state_machine