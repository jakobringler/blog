---
title: Feathers
draft: true
tags:
  - houdini
  - cfx
---
## Concepts

## Creation

## Grooming

### Interpolation

This setup was shared by Jonas Sorgenfrei for his Houdini 20 HIVE talk about feathers. The Vex snippets are very useful and quite reusable:

// primitive wrangle

```C
s[]@templatenames = uniquevals(3, "primitive", "name");

float threshold = 0.00001;

foreach( string templateName; s[]@templatenames )
{
    int prim = nametoprim(2, templateName);
    
    if (prim == -1) 
    {
        push(f[]@templateweights, 0);    
    } 
    else 
    {
        vector uv = primuv(1, "uv", i@skinprim, v@skinprimuv);
        float sample = volumesample(2, templateName, uv);
        sample = (sample>threshold) ? sample: 0;
        push(f[]@templateweights, sample);
    }
}
```

// primitive wrangle

```C
string defaultfeather = chs("default_feather");
string firstName = s[]@templatenames[0]; 

weightarraythreshold(s[]@templatenames, f[]@templateweights, 0.0);

// make sure the default feather is used if the template array is empty
if(len(s[]@templatenames) == 0) 
{
    push(s[]@templatenames, defaultfeather);
    push(f[]@templateweights, 1);
}
```

### 

## Simulation


---

sources / further reading:
- [Feathers - Birds of a Feather: How to make a Feathered Friend | Jonas Sorgenfrei | H20 HIVE](https://www.youtube.com/watch?v=K2Rf1Atetjg)
- [FEATHER NODES - SideFX](https://www.sidefx.com/tutorials/feather-nodes/)
- [New In Houdini 20: Feathers 01 - Creating Feathers](https://www.youtube.com/watch?v=pOjHpX0NOi4)