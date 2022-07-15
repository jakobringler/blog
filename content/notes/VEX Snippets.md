---
title: "VEX Snippets"
tags:
- list
- vex
- houdini
enableToc: true
---

# Wrangle Cheat Sheet
>I try my best to credit and link to any sources. That being said, some of those are pretty old and I have no idea where they came from.
>
>I recommend installing this handy python panel to manage your own snippet collection: [Vex Snippet Library](https://github.com/dchow1992/Vex_Snippet_Library)

### Attribute Min Max
```C
// point wrangle

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

### Attribute to String
```C
s@name = "piece_" + itoa(i@class);
```

### Attribute Transfer
```C
// point wrangle

int posprim;
vector param_uv;
float maxdist = 10;
float dist = xyzdist(1,@P,posprim,param_uv,maxdist);
vector pos = primuv(1,"rest",posprim,param_uv);
v@rest = pos;
```

### Average Point Cloud Positions
```C
// point wrangle

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

### Bias and Gain
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

### Bounding Box
```C
vector bbox = getbbox_size(0);
vector bbox_max = getbbox_max(0);
vector bbox_min = getbbox_min(0);
vector bbox_center = getbbox_center(0);
  
float xsize = getbbox_size(0).x;
float y_max = getbbox_max(0).y;
float z_min = getbbox_min(0).z;
```

### Calculate Point Density
```C
// point wrangle

float maxdist = chf("maxdist");
int maxpts = chi("maxpts");
int points = len(nearpoints, 0, @P, maxdist, maxpts);

f@density = float(points) / maxpts;
```

### Camera Position and Direction
```C
// point wrangle (best used on a single point)

string cam = chs("cam");
matrix camXform = optransform(cam); 
vector cpos;
vector cdir;
vector cup;

cpos = cracktransform(0, 0, 0, {0,0,0}, camXform);
cdir = vtransform(cam,"space:world", {0,0,-1});
cup = vtransform(cam,"space:world", {0,1,0});

v@P = cpos;
v@N = cdir;
v@up = cup;
```

### Create Name Attribute for each Prim Group
```C
string grps[] = detailintrinsic(0, 'primitivegroups');
foreach(string g; grps)
{
	addprimattrib(0, g, 123);
}
```

### Collision Check and Deintersection with SDF VDB
```C
// point wrangle

vector gradient = volumegradient(1, "surface", v@P); 
float surface = volumesample(1, "surface", v@P);

if(surface < chf("dist"))
{
	v@P += normalize(gradient) * abs(surface);
}
```

### Edgefalloff
```C
// point wrangle

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

### Expand Group Over Geo
```C
// point wrangle

int pc = pcopen(0, 'P', @P, chf('radius'), chi('maxpts'));

while (pciterate(pc) > 0)
{
	int currentpt;	
	pcimport(pc, 'point.number', currentpt);
	setpointgroup(0, 'group1', currentpt, 1);
}
```

### Extract Tranformation Matrix
```C
// point wrangle

vector P1 = point(0, "P", 0);
vector P2 = point(0, "P", 1);
vector up = {0,1,0};

vector X = normalize(P2-P1);
vector Z = normalize(cross(up, X));
vector Y = normalize(cross(X, Z));

vector P = P1 + (P2 - P1) / 2;

matrix transform = set(X, Y, Z, P);

setcomp(transform, 0, 0, 3); 
setcomp(transform, 0, 1, 3);
setcomp(transform, 0, 2, 3); 

4@transform = transform;
```

Have a look at [[notes/Matrix Operations |this note]] for more information on how it's used.

### Group by N Connections
```C
// point wrangle

int n = chi("Neighbours");

if (neighbourcount (0, @ptnum) > n)
{
	setpointgroup (0, "grouped", @ptnum, 1);
}
```

### Gravity on Curves (Hanging Cables)
```C
float stiffness = clamp(chf("stiffness"), 0, 0.99);
float u = @curveu * (1 - @curveu) * 4;
u = pow( 1 - u, (1 / (1 - stiffness)));

@P.y *= clamp(fit01(gradient, (1 - ch("gravity")), 1), 0, 1);
```

Sources: 
- [Chris Turner's Tweet](https://twitter.com/allexceptn/status/1488954032425213958)

### Isolate Overlapping Points
```C
// point wrangle

int near[] = pcfind(0, "P", @P, 0.0001, 2);

if(len(near) > 1)
{
	setpointgroup(0, "double", @ptnum, 1);
}
```

### Orientation Template for Copy
```C
// point wrangle

@up = {0,1,0};
@orient = quaternion(maketransform(@N,@up));
vector4 rot_Y = quaternion(radians(ch('Y')),{0,1,0});
@orient = qmultiply(@orient, rot_Y);
```

### Remove Point by Condition
[Mai Ao](https://twitter.com/aomai01) compared two point deletion methods, where method 1 gives a 15x speed increase over the traditional `removepoint()` function

1. group points first and blast group in another step

```C
// point wrangle

float half_cone_rad = radians(chf("half_cone"));
@group_to_delete = acos(dot(@P, {0,0,1})) <= half_cone_rad;
```

2. removepoint()

```C
// point wrangle

float half_cone_rad = radians(chf("half_cone"));

if(acos(dot(@P, {0,0,1})) <= half_cone_rad)
{
	removepoint(0, @ptnum);
}
```

Sources:
- [Mai Ao's Tweet](https://twitter.com/aomai01/status/1514226273794641925/photo/1)

### Remove Point Percentage
```C
// point wrangle

int percentage = ch('percentage'); 

if(@ptnum % 100 < percentage)
{
	removepoint(0, @ptnum );
}
```

### Rotate Vector
```C
// point wrangle

float angle = chf("angle");
vector axis = normalize(chv("axis"));

vector4 rot = quaternion(radians(angle), axis);
@P = qrotate(rot, @P);
```

### Sharpen Point Cloud
```C
// point wrangle

int handle = pcopen(0, "P", @P, chf("radius"), chi("maxpoints"));
v@P = pcfilter(handle, "P");
```