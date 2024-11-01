---
title: "OpenCL"
draft: true
tags:
- houdini
---
## What & Why
## Syntax
### Data Types

### Arithmetic

### Importing Data (Binding)

no `@attrib` stuff for now

first need to bind attribute and tell opencl what it is and what to do

`#bind point &P float3`

`#bind attribtype declarationAttribName datatype`

This imports @P for you

the & tells opencal that it is writeable

```
& writeable
? optional
! readable
```

You can combine these as you wish 

you can also put them before, after or just around the attrib name

``#bind point &P? float3``

optional makes sure your code doesnt error when you dont have the attribute

You can now use `@P` in the code

to write to @P you need to use

```
@P.set(value)
```

attribtypes to bind

```
#bind point
#bind prim
#bind primitive
#bind vertex
#bind detail
#bind layer
#bind global

#bind parm
```

parm allows you to create on the fly parameters like in vex using the different channel expressions:

vex
```
float a = chf("test")
```

opencl
```
#bind parm test float

float a = test;
```

also bindingstab

binding default values

`#bind point &pscale? float val=1`

the `val=` flag lets you set default values if an attribute doesnt exist
### Oddities

getting values from specific point indices

vex
```C
vector pos = point(0, "P", 10);
```

opencl
```C
float3 pos = @P.getAt(10);
```

in vex you go to the point and get the attribute in opencl you use the attribute to get it from another point

works because you already specified in the bind part on top that @P is a point attribute

---

float3 is actually float4 send help pls

---

setting values:
vex:
```C
vector2 uv = set(a,b);
```

opencl:
```C
float2 uv = (floa2)(a,b);
```
## Workflows

### Kernel
### Writeback Kernel

turn on in options checkbox
 important for two step calculation where you want to read the result of an operation back in and compute further in the same node without chaining opencl nodes together

### Function Library

../houdini/ocl/include/

you can import functions from here by using `#include <interpolate.h>`

### Compiled Chains

you can chain opencl sops together and put them into a compiled block which makes them run on the gpu in series without needing to move data back and forth and create overhead

### Defining Things

you can define arbitrary stuff that exists outside of the kernel. when compiling the code the compiler inserts the right pointer that way its more efficient 
only has to be stored once > saves memory

`#define OFFSET (float3)(0.5f)`

it's like a global variable that you can reuse anywhere by just typing `OFFSET`

The definition could also be a function which opens the door for all sorts of shenanigans

```
@define FUNCTION example insert
```

## SOPs & COPs

COPS optional signature tab to set correct image types

---

sources / further reading:
- [OpenCL | Jeff Lait | Houdini 16.5 Masterclass](https://vimeo.com/241568199)
- [OpenCL for VEX users - DOCs](https://www.sidefx.com/docs/houdini/vex/ocl.html)

