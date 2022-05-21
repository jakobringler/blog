---
title: "VEX Snippets"
tags:
- list
enableToc: false
---

### Wrangle Cheat Sheet

> I try my best to credit and link to any sources. That being said, some of those are pretty old and I have no idea where they came from and whether or not they are "mine".

##### Attribute Min Max
```C
float value;
float values[];
string attrname = "name";
float max_value;
float min_value;

for (int i=0; i<@numpt; i++)
{
	value = point(geoself(), attrname, i);
	append(values,value);
}

min_value = min(values);
max_value = max(values);

f[]@range;

@range[0] = min_value;
@range[1] = max_value;
```

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

Sources:
- [Michael Frederickson's Tweet](https://twitter.com/mfrederickson/status/1523148417349816320)
- [Alan Wolfe's Blog Post](https://blog.demofox.org/2012/09/24/bias-and-gain-are-your-friend/)

##### Bounding Box
```C
vector bbox = getbbox_size(0);
vector bbox_max = getbbox_max(0);
vector bbox_min = getbbox_min(0);
vector bbox_center = getbbox_center(0);
  
float xsize = getbbox_size(0).x;
float y_max = getbbox_max(0).y;
float z_min = getbbox_min(0).z;
```

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

##### Group by N Connections
```C
int n = chi("Neighbours");

if (neighbourcount (0, @ptnum) > n)
{
	setpointgroup (0, "grouped", @ptnum, 1);
}
```

##### Isolate Overlapping Points
```C
int near[] = pcfind(0, "P", @P, 0.0001, 2);

if(len(near) > 1)
{
	setpointgroup(0, "double", @ptnum, 1);
}
```

##### Orientation Template for Copy
```C
@up = {0,1,0};
@orient = quaternion(maketransform(@N,@up));
vector4 rot_Y = quaternion(radians(ch('Y')),{0,1,0});
@orient = qmultiply(@orient, rot_Y);
```