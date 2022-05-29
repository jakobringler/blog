---
title: "Matrix Operations"
tags:
- houdini
- math
- vex
---

### coming soon / WIP

### Extracting Transformation Matrix with VEX

##### Creating a Tranformation Matrix

$$
\begin{array}{rcl}
	x-Axis \\
	y-Axis \\
	z-Axis \\
	Position \\
\end{array}
-
\begin{array}{rcl}
	X.x&X.y&X.z&0 \\
	Y.x&Y.y&Y.z&0 \\
	Z.x&Z.y&Z.z&0 \\
	P.x&P.y&P.z&1 \\
\end{array}
$$
We don't really need the fourth column but 3x4 matrices dont "exist". 

```C
matrix transform = set(X, Y, Z, P); // create matrix
```

However this will give us the following matrix with the ones in the fourth column

$$
\Bigg\{\begin{array}{rcl}
	X.x&X.y&X.z&1 \\
	Y.x&Y.y&Y.z&1 \\
	Z.x&Z.y&Z.z&1 \\
	P.x&P.y&P.z&1 \\
\end{array}\Bigg\}
$$

To fix this we can use the setcomp() function.

```C
setcomp(transform, 0, 0, 3); // set row 1 col 4 to 0
setcomp(transform, 0, 1, 3); // set row 2 col 4 to 0
setcomp(transform, 0, 2, 3); // set row 3 col 4 to 0
```



related to:

sources / further reading:
- [Houdini Tutorial | Extracting transformation matrix with VEX - Paweł Rutkowski](https://vimeo.com/284712920)

