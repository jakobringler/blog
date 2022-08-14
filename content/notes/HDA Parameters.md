---
title: "HDA Parameters"
tags:
- houdini
---

### Hiding or Disabling Parameters

each parameter has a 'Disable When' & 'Hide When' option where specific rules can be specified.

Syntax:

{ parametername == VALUE }

also accepts != > < 

### Executing Python Scripts

To write a HDA specific script you can define it in the PythonModule on the scripts page in the type properties. To run it you have to call the following command on any parameter of that HDA in the 'Callback Script' field.

```Python
hou.pwd.FUNCTION()
```
