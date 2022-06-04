---
title: "Numbers"
tags:
- houdini
- vex
- math
---

### Golden Angle

$\phi= 137.5 \degree$  


Also [John Kunz](https://wiki.johnkunz.com/index.php?title=Mathematical_Functions_in_VEX) pointed out these constants that are defined in `$HFS/houdini/vex/include/math.h`  and can be used in any VEX wrangle:

```C++
M_E         2.7182818    // sometimes called the natural number, or Euler's number
LN10        2.3025850    // natural logarithm of 10
LN2         0.6931471    // natural logarithm of 2
LOG10E      0.4342944
LOG2E       1.4426950
PI          3.1415926    // 180° in radians, the ratio of the circumference to diameter of a circle
M_TWO_PI    6.2831852    // 360° in radians, the circumference of a unit circle, 2 * PI
PI_2   	    1.5707963    // 90° in radians, 1/4th of the circumference of a unit circle, PI / 2
PI_4	    0.7853981    // 45° in radians, 1/8th of the circumference of a unit circle, PI / 4
SQRT1_2	    0.7071067
SQRT2	    1.4142135
TOLERANCE   0.0001

// SOURCE: John Kunz's Wiki
```