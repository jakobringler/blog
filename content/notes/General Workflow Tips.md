---
title: "General Workflow Tips"
tags:
- houdini
---

### Temporary Attributes

When creating HDAs or working on bigger setups it might make sense to  prefix any temporary attributes with a special character such as `_` to make removing it later on easier e.g. by using `_*` in an attribute delete SOP. 

By doing so you also ensure that you don't run into issues with incoming attributes that have the same name.


