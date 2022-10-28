---
title: "Attribute Adjust Syntax Tricks"
draft: false
tags:
- houdini
---

## Setting Random Non-continuous Values

You can use the following syntax to specify a range from where the values will be picked. Set the value type to `List of Values`.

```
1-3; 5-9:0.25 
```

This tells the node to create values from 1 to 3 with a stepsize of 1 and values from 5 to 9 with a stepsize of 0.25.

---

sources / further reading:
- [Sexy Explosions | Attila Torok | Houdini 18.5 HIVE](https://www.youtube.com/watch?v=Gxfq9DZTuRM)
- [Houdini DOCs](https://www.sidefx.com/docs/houdini/nodes/sop/attribadjustfloat.html)

