---
title: "Useful Math Functions & Numbers"
tags:
- houdini
- vex
- math
---

### Functions
- [[notes/WaveExpressions |Wave  Expressions]]

---

### Golden Ratio

Ratio between two numbers that equals approximately: 1.618

$\frac{a+b}{a}=\frac{a}{b}$

### Golden Angle

$\phi= 137.5 \degree$ - very useful when creating organic flowery stuff. Have a look at [this](https://entagma.com/td-essentials-create-a-swept-phyllotaxis-operator-in-houdini/) awesome video from Entagma for more information.

$\frac{360\degree -137,5\degree}{137,5\degree}\approx 1.618$

### Fibonacci Sequence

$F_1=F_2=1$       $F_n=F_{n-1} + F_{n-2}$

0  1  1  2  3  5  8  13  21  34  55  89  144  233  377  610  987 ...

---

### VEX Constants

[John Kunz](https://wiki.johnkunz.com/index.php?title=Mathematical_Functions_in_VEX) pointed out these constants that can be used in any VEX wrangle:

```C++
// defined in: $HFS/houdini/vex/include/math.h  

M_E         2.7182818    // Euler's number
LN10        2.3025850    // logarithm of 10
LN2         0.6931471    // logarithm of 2
LOG10E      0.4342944
LOG2E       1.4426950
PI          3.1415926    // 180° in radians
M_TWO_PI    6.2831852    // 360° in radians
PI_2        1.5707963    // 90° in radians
PI_4        0.7853981    // 45° in radians
SQRT1_2     0.7071067
SQRT2       1.4142135
TOLERANCE   0.0001

// SOURCE: John Kunz's Wiki 
```

