---
title: "VEX Snippets"
tags:
- vex
- houdini
enableToc: true
---

## Wrangle Cheat Sheet

>I try my best to credit and link to any sources. That being said, some of those are pretty old and I have no idea where they came from.
>
>I recommend installing [Vex Snippet Library](https://github.com/dchow1992/Vex_Snippet_Library) to manage your own snippet collection. You can find all snippets formatted as .json files ready to be plugged into the library [here](https://github.com/jakobringler/H_VEX_snippets).

> [!check] **Hot Tip:**
> 
> You can find the attribute wrangle node by just typing `aw` ;)

### The Better Cheat Sheets / Snippet Libraries / 101s

- [The Joy of VEX - Matt Estella](https://www.tokeru.com/cgwiki/JoyOfVex)
- [VexCheatSheet - Matt Estella](https://www.tokeru.com/cgwiki/VexCheatSheet)
- [VEX Attribute Glossary - John Kunz](https://wiki.johnkunz.com/index.php?title=VEX_Attribute_Glossary)
- [VEX for artists - Kiryha](https://github.com/kiryha/Houdini/wiki/vex-for-artists)
- [VEX snippets - Kiryha](https://github.com/kiryha/Houdini/wiki/vex-snippets)
- [vex_tutorial - jtomori](https://github.com/jtomori/vex_tutorial)

## VEX 101

### Datatypes
// point wrangle

```C
// Integers
int IntVariable = 1;
i@IntAttribute = 1;

// Floats
float FloatVariable = 0.123;
f@FloatAttribute = 0.123; // attributes will be float by default if you don't specify it otherwise

// Vectors
vector VectorVariable = {0,1,0}; // 3 floats > postion, color, v
vector VectorVariable = set(0.1, 0.2, 0.3); // use the set() function to define a vector with floats 
vector2 Vector2Variable = {0,1} // 2 float > e.g. uvs
vector4 Vector4Vairable =  quaternion(angle, axis); // 4 floats > usually used to store quaternions

v@Vector3Attribute = {0,1,0}; 
u@Vector2Attribute = {0,1};
p@Vector4Attribute = quaternion(angle, axis);

// Matricies
matrix2 Matrix2Variable = {1,2,3,4};
matrix3 Matrix3Variable = {1,2,3,4,5,6,7,8,9};
matrix4 Matrix3Variable = {1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16};

2@Matrix2Attribute = {1,2,3,4};
3@Matrix3Attribute = {1,2,3,4,5,6,7,8,9};
4@Matrix4Attribute = {1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16};

// Arrays
string StringArray[] = {'A','B','C'};
string StringArray[] = array(variable_A, variable_B, variable_C); // use the array() function to define an array with variables 
s[]@StringArrayAttribute = {'A','B','C'};

// Strings
string StringVariable = 'Interesting Text';
s@StringAttribute = 'Interesting Text';

// Dictionaries
dict Dictionary;
Dictionary['key'] = 'value'; // Store 3 in the index key
d@DictAttribute = {}; // dict, can only instantiate as empty type
d@DictAttribute['key'] = 'value'; // can set values once instantiated
```

### Comments

```C
// normal comment

/*
multi-line 
comment
*/
```

### Conversion
// point wrangle

```C
// int to string
int i = 123456;
string s = itoa(i);

// string to int
string s = '123456';
int i = atoi(i);
```

### String manipulation
// point wrangle

```C
// Slicing >>> string[start:end]
string s = 'abcde';
s[:];     // abcde
s[:-1];   // abcd
s[1:];    // bcde
s[1:-1];  // bcd
s[-1];    // e
s[0];     // a

// Concatenation
string text = 'TEXT';
string number = '1234';
string s = sprintf('%s%s%s', text, ' = ', number);
printf('%s', s);
// TEXT = 1234

// Reversing
string s = '1234'
reverse(s);
// 4321
```

### Channel Syntax
// point wrangle

```C
ch('float'); // Float
chf('float2'); // Float
chi('integer'); // Integer
chv('vector'); // Vector 3
chp('quaternion'); // Vector 4 / Quaternion
ch3('matrix3'); // 3x3 Matrix
ch4('matrix4'); // 4x4 Matrix
chs('string'); // String
chramp('ramp', x); // Spline Ramp
vector(chramp('rgbramp', x)); // RGB Ramp
```

### Debug with printf
//point wrangle

```C
// print strings
printf('Hello Houdini');

// print datatypes
printf('string: %s', 'Text'); // string: Text
printf('interger: %d', 123); // integer: 123
printf('float: %f', 0.123456); // float: 0.123456
printf('float (rounded): %.2f', 0.123456); // float (rounded): 0.12

// get all primitives
int primitives[] = expandprimgroup(0, "*");

foreach (int currentPrim; primitives){   
        // print primnum
        printf('Prim %s \n', currentPrim);
        }
```

## Utilities

### Angle between 2 Vectors
// point wrangle 

```C
float angle = degrees(acos(dot(normalize(vector1), normalize(vector2))));
```

### Attribute Min Max
// point wrangle

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
// point wrangle

```C
s@name = "piece_" + itoa(i@class);
```

### Attribute Transfer
// point wrangle

```C
int posprim;
vector param_uv;
float maxdist = 10;
float dist = xyzdist(1,@P,posprim,param_uv,maxdist);
vector pos = primuv(1,"rest",posprim,param_uv);
v@rest = pos;
```

### Average Point Cloud Positions
// point wrangle

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
// point wrangle

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

> [!quote] **Sources:**
> 
> [Michael Frederickson's Tweet](https://twitter.com/mfrederickson/status/1523148417349816320)
> 
> [Alan Wolfe's Blog Post](https://blog.demofox.org/2012/09/24/bias-and-gain-are-your-friend/)

### Bounding Box
// point wrangle

```C
vector bbox = getbbox_size(0);
vector bbox_max = getbbox_max(0);
vector bbox_min = getbbox_min(0);
vector bbox_center = getbbox_center(0);
  
float xsize = getbbox_size(0).x;
float y_max = getbbox_max(0).y;
float z_min = getbbox_min(0).z;
```

### Camera Position and Direction
// point wrangle

```C
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

### Collision Check and Deintersection with SDF VDB
// point wrangle

```C
vector gradient = volumegradient(1, "surface", v@P); 
float surface = volumesample(1, "surface", v@P);

if(surface < chf("dist"))
{
	v@P += normalize(gradient) * abs(surface);
}
```

### Edgefalloff
// point wrangle

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
// point wrangle

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
// point wrangle

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

### Flow Vector around Geometry
// point wrangle

```C
 vector up = chv("up"); // usually 0, 1, 0 for Y  
 vector norm = normalize(v@N);  
 vector c1 = cross(norm, up);  
 vector c2 = cross(norm, c1);  
 v@revolve = c1;
 v@flow = c2;
```

### Follow Surface with Particles using the VDB Gradient
// point pop wrangle

```C
float surf = volumesample(1, "surface", v@P);
vector grad = volumegradient(1, "surface", v@P);

v@P -= normalize(grad) * surf;
```

### Group by N Connections
// point wrangle

```C
int n = chi("Neighbours");

if (neighbourcount (0, @ptnum) > n)
{
	setpointgroup (0, "grouped", @ptnum, 1);
}
```

### Gravity on Curves (Hanging Cables)
// point wrangle

```C
float stiffness = clamp(chf("stiffness"), 0, 0.99);
float u = @curveu * (1 - @curveu) * 4;
u = pow( 1 - u, (1 / (1 - stiffness)));

@P.y *= clamp(fit01(gradient, (1 - ch("gravity")), 1), 0, 1);
```

> [!quote] **Sources:**
> 
> [Chris Turner's Tweet](https://twitter.com/allexceptn/status/1488954032425213958)

### Helix from Line
// point wrangle

```C
  float freq = chf("freq");
  float amp = chf("amp");
  vector pos = @P;
  
  pos.x += sin(@P.y * freq) * amp;
  pos.z += cos(@P.y * freq) * amp;
  @P = pos;
```

### Isolate Overlapping Points
// point wrangle

```C
int near[] = pcfind(0, "P", @P, 0.0001, 2);

if(len(near) > 1)
{
	setpointgroup(0, "double", @ptnum, 1);
}
```

### Name Attribute for each Prim Group
// point wrangle

```C
string grps[] = detailintrinsic(0, 'primitivegroups');
foreach(string g; grps)
{
	addprimattrib(0, g, 123);
}
```

### Normalize @age
// point wrangle

```C
@normage = @age / @life;
```

### Orientation Template for Copy
// point wrangle

```C
@up = {0,1,0};
@orient = quaternion(maketransform(@N,@up));
vector4 rot_Y = quaternion(radians(ch('Y')),{0,1,0});
@orient = qmultiply(@orient, rot_Y);
```

### Point Density
// point wrangle

```C 
float maxdist = chf("maxdist");  
int maxpts = chi("maxpts");  
int points = len(nearpoints(0, @P, maxdist, maxpts));  
  
f@density = float(points) / maxpts;
```

### Remove Point by Condition
// point wrangle

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

> [!quote] **Sources:**
> 
> [Mai Ao's Tweet](https://twitter.com/aomai01/status/1514226273794641925/photo/1)

### Remove Point Percentage
// point wrangle

```C
int percentage = ch('percentage'); 

if(@ptnum % 100 < percentage)
{
	removepoint(0, @ptnum );
}
```

### Remove Point Percentage (ID aware)
// point wrangle

```C
if (haspointattrib(0, "id"))
{    
    if (rand(@id+chi('seed')+0.33)<chf('percentage'))
    {
        removepoint(geoself(), @ptnum);
    }    
}
else
    if (  rand(@ptnum+chi('seed')+0.33)<chf('percentage')) 
    {
        removepoint(geoself(), @ptnum);
    }
```

> [!quote] **Sources:**
> 
> Thanks Hannes!

### Rotate Vector
// point wrangle

```C
float angle = chf("angle");
vector axis = normalize(chv("axis"));

vector4 rot = quaternion(radians(angle), axis);
@P = qrotate(rot, @P);
```

### Sharpen Point Cloud
// point wrangle

```C
int handle = pcopen(0, "P", @P, chf("radius"), chi("maxpoints"));
v@P = pcfilter(handle, "P");
```

### Vector Flow on Objects
// point wrangle

```C
vector pos = v@P * chf("scale");
vector dir = curlnoise2d(pos + @Time * chf("time"));
matrix3 mat = dihedral(set(0,0,1), v@N);
dir *= mat;

v@v = dir;
```

### Velocity Tester
// point wrangle

Very useful to visualize how meshed fluids will move in the motion blur of a rendered frame.

```C
@v *=  chf('vScale'); // Should be 1 by default
float velMax = ch('max_vel'); // Should be a high number by default
float velLength = length(@v);

// Clamp Velocity
if(velLength > velMax)
    {
    @v = normalize(@v)*velMax;
    }
    
// Export Speed
if (chi("export_Speed")==1)
    {
    @speed = length(@v);
    }

// Test Velocity
@P += @v/$FPS*chf('velTester');
```

> [!quote] **Sources:**
> 
> Thanks Hannes!

### Wave Expressions

[[notes/WaveExpressions |Summary]] of different useful periodic functions like square or sawtooth

