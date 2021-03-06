# 🎥 OpenGL Mathematics (glm) for `C`

The original glm library is for C++ only (templates, namespaces, classes...), this library targeted to C99 but currently you can use it for C89 safely by language extensions e.g `__register`

#### Note for C++ developers:
If you don't aware about original GLM library yet, you may also want to look at:
https://github.com/g-truc/glm


<table>
  <tbody>
    <tr>
      <td>
        <div>Like some other graphics libraries (especially OpenGL) this library use Column-Major layout to keep matrices in the memory. </div>
        <div>&nbsp;</div>
        <div>In the future the library may support an option to use row-major layout, CURRENTLY if you need to row-major layout you will need to transpose it. </div>
      </td>
      <td>
        <img src="https://upload.wikimedia.org/wikipedia/commons/3/3f/Matrix_Columns.svg" width="300px" />
      </td>
    </tr>
  </tbody>
</table>

##Features
- general purpose matrix operations (mat4, mat3)
- chain matrix multiplication (square only)
- general purpose vector operations (cross, dot, rotate, proj, angle...)
- affine transforms
- matrix decomposition (extract rotation, scaling factor)
- optimized affine transform matrices (mul, rigid-body inverse)
- camera (lookat)
- projections (ortho, perspective)
- quaternions
- euler angles / yaw-pitch-roll to matrix
- extract euler angles
- inline or pre-compiled function call

<hr />

You have two option to call a function/operation: inline or library call (link)
Almost all functions are marked inline (always_inline) so compiler probably will inline. 
To call pre-compiled version, just use `glmc_` (c stands for 'call') instead of `glm_`. 

```C
  #include <cglm.h>        /* for inline */
  #include <cglm-call.h>   /* for library call (this also includes cglm.h) */

  mat4 rot, trans, rt;
  /* ... */
  glm_mul(trans, rot, rt);  /* inline */
  glmc_mul(trans, rot, rt); /* call from library */
```
Most of math functions are optimized manualy with SSE2 if available, if not? Dont worry there are non-sse versions of all operations

You can pass matrices and vectors as array to functions rather than get address.

```C
  mat4 m = {
    1, 0, 0, 0,
    0, 1, 0, 0,
    0, 0, 1, 0,
    0, 0, 0, 1
  };

  glm_translate(m, (vec3){1.0f, 0.0f, 0.0f});
```

Library contains general purpose mat4 mul and inverse functions but also contains some special form (optimized) of these functions for affine transform matrices. If you want to multiply two affine transform matrices you can use glm_mul instead of glm_mat4_mul and glm_inv_tr (ROT + TR) instead glm_mat4_inv
```C
/* multiplication */
mat4 modelMat;
glm_mul(T, R, modelMat);

/* othonormal rot + tr matrix inverse (rigid-body) */
glm_inv_tr(modelMat);
```

##License
MIT. check the LICENSE file

## Build

### Unix (Autotools)

```text
$ sh autogen.sh
$ ./configure
$ make
$ make install
$ [sudo] make install
```

### Windows (MSBuild)

```text
TODO:
```
##How to use
If you want to use inline versions of funcstions then; include main header
```C
#include <cglm.h>
```
the haeder will include all headers. Then call func you want e.g. rotate vector by axis:
```C
glm_vec_rotate(v1, glm_rad(45), (vec3){1.0f, 0.0f, 0.0f});
```
some functions are overloaded :) e.g you can normalize vector:
```C
glm_vec_normalize(vec);
```
this will normalize vec and store normalized vector into `vec` but if you will store normalized vector into another vector do this:
```C
glm_vec_normalize_to(vec, result);
```
like this function you may see `_to` postfix, this functions store results to another variables and save temp memory


to call pre-compiled versions include header with `c` postfix, c means call. Pre-compiled versions are just wrappers.
```C
#include <cglm-call.h>
```
this header will include all heaers with c postfix. You need to call functions with c posfix:
```C
glmc_vec_normalize(vec);
```

Function usage and parameters are documented inside related headers.
##Notes

- This library uses float types only, does not support Integers, Double... yet
- If headers are not working properly with your compiler, IDE please open an issue, because I'm using GCC and clang to test it maybe sometimes MSVC
