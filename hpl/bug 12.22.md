#### 6.1.2* Strange bug when `LAinc = -include /usr/include/x86_64-linux-gnu/cblas-openblas.h`

* Reproduction

    If you use this config:

    ```
    LAdir        = /usr/lib/x86_64-linux-gnu/openblas
    LAinc        = -include /usr/include/x86_64-linux-gnu/cblas-openblas.h
    LAlib        = -Wl,-rpath=$(LAdir)/ $(LAdir)/libblas.so
    ```

    The build will fail because the error below: 

    ```
    [0]Lab-XPS@levi:~/Desktop/ap-collection/test/hpl-2.3/src/pauxil/linux$ mpicc -c -DAdd_ -DF77_INTEGER=int -DStringSunStyle  -I../../../include -I../../../include/linux -include /usr/include/x86_64-linux-gnu/cblas-openblas.h -I /usr/include/mpich/    ../HPL_infog2l.c -o HPL_infog2l.o
    
    In file included from /usr/include/x86_64-linux-gnu/openblas_config.h:104:0,
                     from /usr/include/x86_64-linux-gnu/cblas-openblas.h:5,
                     from <command-line>:0:
    ../HPL_infog2l.c:54:37: error: expected ‘)’ before ‘__extension__’
        int                              I,
                                         ^
    ../HPL_infog2l.c:55:4: error: expected ‘;’, ‘,’ or ‘)’ before ‘int’
        int                              J,
        ^~~
    ```

    

* Explanation:

[github: openssl issues 4022](https://github.com/openssl/openssl/issues/4022)

```c
// /usr/include/x86_64-linux-gnu/cblas-openblas.h
#include "openblas_config.h"

// /usr/include/x86_64-linux-gnu/openblas_config.h
  #include <complex.h>     // line 104

// /usr/include/complex.h
#define _Complex_I  (__extension__ 1.0iF)
#undef I 
#define I _Complex_I
// no #undef I
```

```c
// hpl-2.3/src/pauxil/linux$ mpicc -c -DAdd_ -DF77_INTEGER=int -DStringSunStyle  -I../../../include -I../../../include/linux -include /usr/include/x86_64-linux-gnu/cblas-openblas.h -I /usr/include/mpich/    ../HPL_infog2l.c -E
void HPL_infog2l
(
   int 
# 54 "../HPL_infog2l.c" 3 4
                                   (__extension__ 1.0iF)
# 54 "../HPL_infog2l.c"
                                    ,
   int J,
   const int IMB,
   const int MB,
   const int INB,
   const int NB,
   const int RSRC,
   const int CSRC,
   const int MYROW,
   const int MYCOL,
   const int NPROW,
   const int NPCOL,
   int * II,
   int * JJ,
   int * PROW,
   int * PCOL
)

```



* Solution 1:

```c
// hpl-2.3/include/hpl.h
#undef I     // add this at the end of the file
```

* Solution 2:

    Now I notice that HPL use `#include "hpl_blas.h"`, where define all the blas function it need. So, HPL don't need to link header in `/usr/include/x86_64-linux-gnu/`

    Just `LAinc        = ` and everything solved.