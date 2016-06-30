
# Improving the cmd(.h/.cpp) module

## Code Review

Every programmer has written bad code in there life. I will discuss what I did
and why it was bad. Then I will write my new revision. The two codes are not 
completely compatible and one will not work as a substitute.

The first major mistake was the use of a static array in the header file. This
is a major issue! A single copy of this static variable will exist everywhere
those lines exist. The issue arises because everywhere "cmd.h" is *#included*
those lines will be inserted. The array was initially suppose to insure only
one copy of each string existed. Instead, it caused 2 or 3 copies to exist.

The second major mistake is that I included all the addon functionality into
the "cmd.h/cpp" files. This required multiple headers to be included. All of the
commands that could be executed has to exist in the "cmd.cpp" file.

The third was a strange (and possibly improper) use of *Enum*. Basiclly they were
used as a "const int" that existed inside a namespace so that it could be clear
what it related to.

## Revisions

Using a *struct* to combine the data previous stored in three arrays (technically
the Enum wasn't an array but rather the index). The struct was declared, and
defined in the "cmd.h". An array of the data was defined in "cmd.cpp" with a
clearly defined max size.

The external functionality was exported to other files (some not fully working 
yet), such as "cmd\_particle(.h/.cpp)" or "cmd\_rgb(.h/.cpp)". The "cmd(.h/.cpp)"
focused on ways to get, add, and execute the commands in the list.

Note that the global(.h/.cpp) was created to hold the device constants that should
be defined across all modules. It is not important for this implementation but
is utilized by cmd\_particle.

## Future work

I have learned that the particle microcontroller's firmware is compiled with the
newer c++11 standard. I would like to use a *std::vector* to hold the commands.
I would like to use the uid of the commands to *get* them, and not the index of
the array. Then I would like for new commands to be added by using the push_back
function of the vector.


## Files

### Old Files
```json
{"particle-old-cmd": {
    ".": {
        "path": "./particle-old-cmd"
    }, 
    "cmd.cpp": {
        "path": "./particle-old-cmd/cmd.cpp", 
        "sha1": "209665290b1a9bbce88267517a48a58daadf031d"
    }, 
    "cmd.h": {
        "path": "./particle-old-cmd/cmd.h", 
        "sha1": "f65d7fa078e920601bc8b1ef54bd58b5c30ff6e8"
    }
}}
```

### New Files
```json
{"particle-cmd-v1": {
    ".": {
        "path": "./particle-cmd-v1"
    }, 
    "cmd": {
        ".": {
            "path": "./particle-cmd-v1/cmd"
        }, 
        "cmd.cpp": {
            "path": "./particle-cmd-v1/cmd/cmd.cpp", 
            "sha1": "ced92ee71164e8c2b5c7aa4876e16240467b7f99"
        }, 
        "cmd.h": {
            "path": "./particle-cmd-v1/cmd/cmd.h", 
            "sha1": "5a50706f80d10bcc9d511e01e5347ecca1a6e591"
        }, 
        "cmd_particle.cpp": {
            "path": "./particle-cmd-v1/cmd/cmd_particle.cpp", 
            "sha1": "e10981972006b6f5cdc1af067a0a1aa40fbdbbbe"
        }, 
        "cmd_particle.h": {
            "path": "./particle-cmd-v1/cmd/cmd_particle.h", 
            "sha1": "0c7a3c903d0ac0b4dfc04b60bd957b03e944431b"
        }, 
        "cmd_rgb.cpp": {
            "path": "./particle-cmd-v1/cmd/cmd_rgb.cpp", 
            "sha1": "c1bb55529a5440fd6e3db79eb678c2ffefba6d97"
        }, 
        "cmd_rgb.h": {
            "path": "./particle-cmd-v1/cmd/cmd_rgb.h", 
            "sha1": "43704fa2ba328574c47909d3245d26930ac865ba"
        }
    }, 
    "global.cpp": {
        "path": "./particle-cmd-v1/global.cpp", 
        "sha1": "adc83b19e793491b1c6ea0fd8b46cd9f32e592fc"
    }, 
    "global.h": {
        "path": "./particle-cmd-v1/global.h", 
        "sha1": "c5d246215bca8d62bcedab1e9fca4700acae101a"
    }, 
    "main.ino": {
        "path": "./particle-cmd-v1/main.ino", 
        "sha1": "20174b47b4413106695c3434df83f69849c4b3f7"
    }, 
    "rgb_and_particle_cmds.bash": {
        "path": "./particle-cmd-v1/rgb_and_particle_cmds.bash", 
        "sha1": "92c06c8876d03201a08243e839de98790f208c0f"
    }
}}
```

