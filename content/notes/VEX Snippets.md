---
title: "VEX Snippets"
tags:
- list
---

### Wrangle Cheat Sheet

##### Attribute to String
```VEX
s@name = "piece_" + itoa(i@class);
```

##### Attribute Transfer
```VEX
int posprim;
vector param_uv;
float maxdist = 10;
float dist = xyzdist(1,@P,posprim,param_uv,maxdist);
vector pos = primuv(1,"rest",posprim,param_uv);
v@rest = pos;
```

##### Average Point Cloud Positions
```VEX
vector value;
vector values[];

for (int i=0; i<@numpt; i++)
{
	value = point(geoself(), "P", i);
	append(values,value);
}
  
vector avgP = avg(values);

if(@ptnum>0)
{
	removepoint(geoself(), @ptnum);
} 

@P = avgP;
```

##### Bias and Gain
```VEX
function float bias(float val; float bias) 
{
    return (val / ((((1.0/bias) - 2.0)*(1.0 - val))+1.0));
}

function float gain(float val; float gain) 
{
    if(val < 0.5)
    {
        return bias(val * 2.0,gain)/2.0;
    }
    else
    {
        return bias(val * 2.0 - 1.0,1.0 - gain)/2.0 + 0.5;
    }
}

float val = pow(val, exp); 
```

source:
- https://twitter.com/mfrederickson/status/1523148417349816320
- https://blog.demofox.org/2012/09/24/bias-and-gain-are-your-friend/

##### Calculate Point Density
```VEX
float maxdist = chf("maxdist");
int maxpts = chi("maxpts");
int points = len(nearpoints, 0, @P, maxdist, maxpts);
f@density = float(points) / maxpts;
```

##### Create Name Attribute for each Prim Group
```VEX
string grps[] = detailintrinsic(0, 'primitivegroups');
foreach(string g; grps)
{
	addprimattrib(0, g, 123);
}
```


related to: [[notes/Houdini |Houdini]] 

