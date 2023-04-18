# xlsl-cpp-interop
Code that makes the exchange between HLSL/GLSL and C++ easier

## Share Structs
Before including the file below define GLSL/HLSL for usage in `GLSL` or `HLSL` and define nothing or `GLM` for usage in C/C++ optional with glm
```c++
// example of HLSL/C++ interopt
#ifndef __cplusplus
#define HLSL
#endif
```
```c++
#include "xlsl_type_conv_def.h"
STRUCT (
    MAT4    (viewMat)
    MAT4    (projMat)
    VEC4    (cameraPos)
    FLOAT   (ditherStrength)
,GlobalUbo)
#include "xlsl_type_conv_undef.h"
// GlobalUbo can now be used in C++ and HLSL/GLSL by including this code
```

## Share Code
The only change needed for this to work in HLSL/GLSL is to replace the `in/out/inout` qualifiers with `IREF/OREF/REF`. 

> [GLM](https://glm.g-truc.net/) is required for this.
```c++
/* HLSL/GLSL */

void addOne(inout float f) { f += 1; }
// turns into 
#include "c_adapter.h"
void addOne(REF(float) f) { f += 1; }
```
```c++
/* C++ */
#include "hlsl_adapter.h" // advice: rather not include this globaly
#include "your.hlsl"

float f;
addOne(f);
```

> No excuses anymore, go and write some unit tests for your GPU code. 