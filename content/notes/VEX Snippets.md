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
int posprim;
vector param_uv;
float maxdist = 10;
float dist = xyzdist(1,@P,posprim,param_uv,maxdist);
vector pos = primuv(1,"rest",posprim,param_uv);
v@rest = pos;
```

### Average Point Cloud Positions
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
float maxdist = chf("maxdist");
int maxpts = chi("maxpts");
int points = len(nearpoints, 0, @P, maxdist, maxpts);

f@density = float(points) / maxpts;
```

### Create Name Attribute for each Prim Group
```C
string grps[] = detailintrinsic(0, 'primitivegroups');
foreach(string g; grps)
{
	addprimattrib(0, g, 123);
}
```

### Edgefalloff
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

### Expand Group Over Geo
```C
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
int n = chi("Neighbours");

if (neighbourcount (0, @ptnum) > n)
{
	setpointgroup (0, "grouped", @ptnum, 1);
}
```

### Isolate Overlapping Points
```C
int near[] = pcfind(0, "P", @P, 0.0001, 2);

if(len(near) > 1)
{
	setpointgroup(0, "double", @ptnum, 1);
}
```

### Orientation Template for Copy
```C
@up = {0,1,0};
@orient = quaternion(maketransform(@N,@up));
vector4 rot_Y = quaternion(radians(ch('Y')),{0,1,0});
@orient = qmultiply(@orient, rot_Y);
```

### Remove Point by Condition
[Mai Ao](https://twitter.com/aomai01) compared two point deletion methods, where method 1 gives a 15x speed increase over the traditional `removepoint()` function

1. group points first and blast group in another step

```C
float half_cone_rad = radians(chf("half_cone"));
@group_to_delete = acos(dot(@P, {0,0,1})) <= half_cone_rad;
```

2. removepoint()

```C
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
int percentage = ch('percentage'); 

if(@ptnum % 100 < percentage)
{
	removepoint(0, @ptnum );
}
```

### Translate, Rotate, Scale & Bend
```C
vector t = chv('translate');
vector r = radians(chv('rotate_before'));
vector rot = radians(chv('rotate_after'));
vector s = chv('scale');
matrix xform = detail(0,'xform');

if(determinant(xform) == 0) xform = ident(); //initialize xform if not found

//Scaling
matrix3 S = set(set(s.x, 0, 0), set(0, s.y, 0), set(0, 0, s.z));

//General Rotation Before
matrix3 R = set( cos(r.b)*cos(r.g), cos(r.b)*sin(r.g)*sin(r.r)-sin(r.b)*cos(r.r), cos(r.b)*sin(r.g)*cos(r.r)+sin(r.b)*sin(r.r),
                 sin(r.b)*cos(r.g), sin(r.b)*sin(r.g)*sin(r.r)+cos(r.b)*cos(r.r), sin(r.b)*sin(r.g)*cos(r.r)-cos(r.b)*sin(r.r),
                 -sin(r.g), cos(r.g)*sin(r.r), cos(r.g)*cos(r.r) );

//Translation
matrix T = set(set(1, 0, 0, 0), set(0, 1, 0, 0), set(0, 0, 1, 0), set(t.x, t.y, t.z, 1));

//Basic Rotation After

matrix3 Rx = set(1,0,0, 0,cos(rot.x),-sin(rot.x), 0,sin(rot.x),cos(rot.x));
matrix3 Ry = set(cos(rot.y),0,sin(rot.y), 0,1,0, -sin(rot.y),0,cos(rot.y));
matrix3 Rz = set(cos(rot.z),-sin(rot.z),0, sin(rot.z),cos(rot.z),0 ,0,0,1);
matrix3 Rxyz = Rx*Ry*Rz;

//Bend

vector bendaxis = chv('Bend_Axis');
vector min = getbbox_min(0);
vector max = getbbox_max(0);
float grad;

if(chi('bend_coord') == 0)
{
    grad = fit(@P.x, min.x, max.x, 0, 1);
}

else if(chi('bend_coord') == 1)
{
    grad = fit(@P.y, min.y, max.y, 0, 1);
}

else
{
    grad = fit(@P.z, min.z, max.z, 0, 1);
}

float angle = chf('bend') *pow(chramp('BendRamp',grad), 1+chf('bend_power'));
vector4 quat = quaternion(radians(angle), bendaxis);
@P = qrotate(quat, @P);

//Rotate around axis at Centroid

matrix mtx1 = ident(); 
vector axis = normalize(chv('axis'));
float ang = radians(chf('angle'));
rotate(mtx1, ang, axis);

//Matrix Multiplication

matrix mtx2 = invert(xform)*S*mtx1*xform*R*T*Rxyz; //order matters
@P *= mtx2;

//store in xform detail

setdetailattrib(0, 'xform', xform*mtx2, 'set');
```

Sources:
- [Houdini Translate Rotate Scale Bend with Matrices & Quaternions in VEX - Nodes Of Nature](https://www.youtube.com/watch?v=e9qLWS2La28)
- [Mohamad Salame's ArtStation](https://www.artstation.com/blogs/mohamad_salame1/OQNX/translate-rotate-scale-bend-with-matrices-quaternions-in-vex)