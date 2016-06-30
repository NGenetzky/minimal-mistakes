
# Customizing Firmware Cloud API

## Previous dicussed
The do, put, and post capability was previously dicussed in
[using_particle_cloud.md][1]. That discussion focused on how
I believed they were useful and how I had used them.

## Introduction

I will discuss the infastructure I created to support the capability. How its
behavior relates to the functionality provided by the orignial API. Then I will
provide figurative use cases.

## Infastructure

### node

The primary function provided by **node** is to link a particular string of
characters with a figurative node. Currently a node is simply defined by
a unique identifier. The internal implementation may change but it should not
affect the way other files use the functions provided by this class. 

The primary public functions of this module are:
```cpp
typdef unsigned int Id;
void add_node(Id uid, char *name);
Id node_lookup(char *s);
```
The data is internally stored as:
```cpp
struct Node {
    Id uid;
    char path[MAX_PATH_LENGTH];
}
std::vector<Node> nodes;
```
The internal storage should be abstracted from the implemntation. Although the
nodes are stored as a vector it may be better to think of them as a **std::map**
with different keys when its being editied and when its being used.

When using node_lookup the key of the map can be considred to be the **name** of
the node, but when adding or editing the nodes in the vector then the key should
be considered the **uid**.

### cmd

Do is used to call functions by name with no arguments. It is more limited than a
than a function. This module was written before node, and node was an improvement
and an abstraction from cmd. Now the cmd module could be refactored to use node
and it would become much shorter.

Externally the behavior should be unchanged by the refactoring. If the
**do function** is called with an argument string that matches the name of
a node then the do function of the node would be called.

### put

Put is used to call functions by name with string of characters as an argument.
Each put is assigned to a particular node by the nodes **Id**. Then when the
**put function** is called with the args String containing the *key* and the *value*
separated by an equal sign then the put function associate with the key name is called
and *value* is supplied as an argument.

The internal storage is a map with the Id as the key and the function pointer
as the value. Storage in this data structure matches the conceptual view but
it may be more efficient to use a vector. 

```cpp
typedef int PutF (char *args);
std::map<Id, PutF> puts;
```

## Put as a replacement for function

The Particle firmware allows for 12 functions to be defined. Sometimes this is
insuccicient. The put function can be used as a drop-in replacement for
other functions. The only limitation it adds is that the length of the string
argument reduced by the length of the function name plus one.

Using function:
```cpp
int code_name(String args); // declare the function
particle.function("public_name", code_name); // expose the funciton in setup
```
```bash
particle call DeviceName public_name "arguments"
```

Using put:
```cpp
int code_name(char *args); // declare the function
set_node(100, "public_name");
add_put(100, code_name);
```
```bash
particle call DeviceName put "public_name=arguments"
```

## Examples of Commands

### Switches: LED, Relay or Digital Pin

Simple commands could be used to control and monitor a switch. 
The command module is very effective for controlling a digital pin. Commands
such as: "on", "off", and "toggle" cover the majority of the actions required. 

## Action: Setup module, Start Timer or Publish value

Commands can be created for common routines. Setting up communication or other
 modules is a perfect example of common routines. It is also helpful when an
action needs to be repeated at a certain interval.



[1]: 2016/06/08/using_particle_cloud.md
