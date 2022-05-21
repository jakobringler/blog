---
title: "VEX Snippets"
tags:
- list
enableToc: false
---

### Wrangle Cheat Sheet

##### Attribute to String
```C
s@name = "piece_" + itoa(i@class);
```

##### Attribute Transfer
```C
int posprim;
vector param_uv;
float maxdist = 10;
float dist = xyzdist(1,@P,posprim,param_uv,maxdist);
vector pos = primuv(1,"rest",posprim,param_uv);
v@rest = pos;
```

##### Average Point Cloud Positions
```C
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
```C
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

sources:
- [Michael Frederickson's Tweet](https://twitter.com/mfrederickson/status/1523148417349816320)
- [Alan Wolfe's Blog Post](https://blog.demofox.org/2012/09/24/bias-and-gain-are-your-friend/)

##### Calculate Point Density
```C
float maxdist = chf("maxdist");
int maxpts = chi("maxpts");
int points = len(nearpoints, 0, @P, maxdist, maxpts);

f@density = float(points) / maxpts;
```

##### Create Name Attribute for each Prim Group
```C
string grps[] = detailintrinsic(0, 'primitivegroups');
foreach(string g; grps)
{
	addprimattrib(0, g, 123);
}
```

##### Edgefalloff
```C
if (@edgefalloff==1)
{
	int near[] = nearpoints(0,@P,chf("dist"));
	
	foreach (int pnt;near)
	{
		vector pntP = point(0,"P",pnt);
		float dist = fit(distance(pntP,@P),0,chf("dist")*2,1,-1);
		setpointattrib(0,"edgefalloff",pnt,dist,"set");
	}
}
```

##### Expand Group Over Geo

```C
int pc = pcopen(0, 'P', @P, chf('radius'), chi('maxpts'));

while (pciterate(pc) > 0)
{
	int currentpt;	
	pcimport(pc, 'point.number', currentpt);
	setpointgroup(0, 'group1', currentpt, 1);
}
```
