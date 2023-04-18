# xlsl-cpp-interop
Helper for making the exchange of code between HLSL/GLSL and C++ easier

## Share Structs
Before including the file below in your GLSL/HLSL code either define `GLSL` or `HLSL`\
Before including the file below in your C++ code using [glm](https://glm.g-truc.net/) define `GLM`\
For pure C/C++ usage nothing needs to be defined
```c++
// example of HLSL/C++ interopt
#ifndef __cplusplus
#define HLSL
#endif

#include "xlsl_type_conv_def.h"
STRUCT (
    MAT4    (viewMat)
    MAT4    (projMat)
    VEC4    (cameraPos)
    FLOAT   (ditherStrength)
,GlobalUbo)
#include "xlsl_type_conv_undef.h"
// GlobalUbo can now be used in C++ and HLSL by including the file including this code
```

## Share Code
The only change needed for this to work with your existing HLSL/GLSL code is to replace the `in/out/inout` qualifiers with `IREF/OREF/REF`. 

> [glm](https://glm.g-truc.net/) is required for this

> Not every built-in function of HLSL/GLSL is yet covered but it should be straightforward to extend
```c++
/* HLSL/GLSL */
void addOne(inout float f) { f += 1; }
// turns into 
#include "c_adapter.h"
void addOne(REF(float) f) { f += 1; }
```
```c++
/* C++ */
#include "hlsl_adapter.h" // advice: rather not include this globally
#include "your.hlsl"

int main(int argc, char* argv[]) {
  float f;
  addOne(f);
  return 0;
}
```

> No excuses anymore, go and write some unit tests for your GPU code. 
