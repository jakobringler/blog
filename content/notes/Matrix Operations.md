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
	\color{red} x-Axis \\
	\color{green} y-Axis \\
	\color{blue} z-Axis \\
	\color{yellow} Position \\
\end{array}
\equiv
\begin{array}{rcl}
	\color{red} 1&\color{red}0&\color{red}0&0 \\
	\color{green}0&\color{green}1&\color{green}0&0 \\
	\color{blue}0&\color{blue}0&\color{blue}1&0 \\
	\color{yellow}0&\color{yellow}0&\color{yellow}0&1 \\
\end{array}
$$

We don't really need the fourth column but 3x4 matrices dont "exist". 

```C
matrix transform = set(X, Y, Z, P); // create matrix
```

However this will give us the following matrix with the ones in the fourth column

$$
\begin{array}{rcl}
	\color{red} X.x&\color{red}X.y&\color{red}X.z&1 \\
	\color{green} Y.x&\color{green}Y.y&\color{green}Y.z&1 \\
	\color{blue} Z.x&\color{blue}Z.y&\color{blue}Z.z&1 \\
	\color{yellow} P.x&\color{yellow}P.y&\color{yellow}P.z&1 \\
\end{array}
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
- [Houdini Translate Rotate Scale Bend with Matrices & Quaternions in VEX - Nodes of Nature](https://www.youtube.com/watch?v=e9qLWS2La28)

