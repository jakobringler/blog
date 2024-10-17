---
title: Misery Of OpenCL 02
draft: true
tags:
  - houdini
  - opencl
---
## Basic Syntax and Datatypes

| VEX                | OpenCL             | Type                                                                                                                                                                                                           |
| ------------------ | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| int                | int                | Integer value                                                                                                                                                                                                  |
| `--`               | int2/3/4/8/16      | Integer vector of the given number of components.                                                                                                                                                              |
| float              | float              | Float value                                                                                                                                                                                                    |
| vector2            | float2             | Two component vector                                                                                                                                                                                           |
| vector             | float3             | Three component vector. Note that in OpenCL this is laid out in memory with four floats, so care has to be taken if used in a structure.                                                                       |
| vector4            | float4             | Four component vector.                                                                                                                                                                                         |
| `--`               | float8/16          | OpenCL also has native 8 and 16 component vectors.                                                                                                                                                             |
| matrix2            | float4, mat2       | A float4 is the same size as a matrix2, but we've defined a mat2 to make the semantics more clear and provide some utility functions.                                                                          |
| matrix3            | mat3               | There is no native 9-element type. The mat3 we have defined is actually a 12-element type, so care has to be done to move it to or from memory.                                                                |
| matrix             | float16, mat4      | A float16 is the same size as a matrix, but we've defined a mat4 to make the semantics clear and provide some utility functions.                                                                               |
| string             | `--`               | You can define string literals for things like `printf()` in OpenCL, but as this is on the GPU there are many oddities, so these are not the same as the `const char *` you may expect from other C languages. |
| dict               | `--`               | Dictionaries aren’t supported in OpenCL.                                                                                                                                                                       |
| `int[]`, `float[]` | `int *`, `float *` | VEX has an array type which can grow and shrink dynamically. In OpenCL, pointers can be used to a similar effect, but cannot change dynamically.                                                               |

```C
#bind point &P float3

@KERNEL
{
    float3 pos = @P;
    pos += 1;
    @P.set(pos);
}
```


---

sources / further reading:
- 

