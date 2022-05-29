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
	1&1&1 \\
	1&1&1 \\
	1&1&1 \\
\end{array}
$$

$$
\,\begin{array}{rcl}
	\color{x} x-Axis \\
	\color{y} y-Axis \\
	\color{z} z-Axis \\
	\color{p} Position \\
	empty
\end{array}\,
-
\Bigg\{\,\begin{array}{rcl}
	\color{x} 1&\color{x}0&\color{x}0&0 \\
	\color{y}0&\color{y}1&\color{y}0&0 \\
	\color{z}0&\color{z}0&\color{z}1&0 \\
	\color{p}0&\color{p}0&\color{p}0&1 \\
\end{array}\,\Bigg\}
$$


```C
matrix transform = set(X, Y, Z, P); // create matrix
```

$$
\Bigg\{\,\begin{array}{rcl}
	\color{x} X.x&\color{x}X.y&\color{x}X.z&1 \\
	\color{y} Y.x&\color{y}Y.y&\color{y}Y.z&1 \\
	\color{z} Z.x&\color{z}Z.y&\color{z}Z.z&1 \\
	\color{p} P.x&\color{p}P.y&\color{p}P.z&1 \\
\end{array}\,\Bigg\}
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

