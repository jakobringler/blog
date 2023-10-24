---
title: "Solaris Attribute Conversion"
draft: false
tags:
- houdini
- solaris
---
### USD Translations for Common Houdini Attributes

```C
v@P      >>> points 
v@Cd     >>> primvars:displayColor 
f@Alpha  >>> primvars:displayOpacity 
i@id     >>> ids
v@uv     >>> primvars:st // think of STMaps in Nuke
v@N      >>> normals 
v@v      >>> velocities 
v@w      >>> angularVelocities
v@accel  >>> accelerations 
f@width,
f@widths,
f@pscale >>> widths
```

---

sources / further reading:
- [Converting attributes - DOCs](https://www.sidefx.com/docs/houdini/solaris/sop_import.html#attrs)

