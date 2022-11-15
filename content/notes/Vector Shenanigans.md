---
title: "Vector Shenanigans"
draft: false
tags:
- houdini
- math
- vex
---

### Vector Flow on Objects

![[notes/images/vectorflowonobject.png]]

```C
vector pos = v@P * chf("scale");
vector dir = curlnoise2d(pos + @Time * chf("time"));
matrix3 mat = dihedral(set(0,0,1), v@N);
dir *= mat;

v@v = dir;
```

---

- related / sources: