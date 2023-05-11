---
title: "Strange Attractors"
draft: false
tags:
- houdini
- math
---

![[notes/images/strangeattractors.png]]

//pointwrangle "starting_conditions" (used the values in each comment)

```C
@a = chf("sigma"); // 10
@b = chf("rho");   // 28
@c = chf("beta");  // 8/3
@dt = ch("dt");    // 0.002
```

The solver is just a point wrangle that moves the starting points along based on the formula and starting conditions.

// the solver

![[notes/images/strangeattractors_solver.png]]

//pointwrangle "move_points"

```C
float x = @P.x;
float y = @P.y;
float z = @P.z;

float dx = (@a * ( y - x )) * @dt;
float dy = (x * ( @b - z ) - y ) * @dt;
float dz = (x * y - @c * z ) * @dt;

@P.x += dx;
@P.y += dy;
@P.z += dz;
```

---

sources / further reading:
- [Darstellung von Attraktoren mit Cinema - 3d Meier](http://www.3d-meier.de/)
- [Houdini Tutorial 0034 Strange Attractor (Slow version) - Junichiro Horikawa](https://www.youtube.com/watch?v=saA6-edb-OE)
- [VEX in Houdini: Strange Attractors - Entagma](https://www.youtube.com/watch?v=vl1tLebo-xY)
- [Coding Challenge #12: The Lorenz Attractor in Processing - The Coding Train](https://www.youtube.com/watch?v=f0lkz2gSsIk)
- [Chaos: The Science of the Butterfly Effect - Veritasium](https://www.youtube.com/watch?v=fDek6cYijxI)