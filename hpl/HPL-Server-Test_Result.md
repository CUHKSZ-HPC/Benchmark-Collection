## Server Single CPU Test

`Intel(R) Xeon(R) Silver 4210 CPU @ 2.20GHz, Cascade Lake`

```bash
HPLinpack benchmark input file
Innovative Computing Laboratory, University of Tennessee
HPL.out      output file name (if any)
6            device out (6=stdout,7=stderr,file)
1            # of problems sizes (N)
12000         Ns
1            # of NBs
256          NBs
0            PMAP process mapping (0=Row-,1=Column-major)
1            # of process grids (P x Q)
1            Ps
1            Qs
# The others are same as HPL-XPS-Test_Result.md
```



| Gflops      | OpenMPI | MPICH | Intel MPI  |
| ----------- | ------- | ----- | ---------- |
| netlib-blas |         |       |            |
| atlas       |         |       |            |
| openblas    |         |       |            |
| Intel MKL   |         |       | 2.9405e+01 |

* When HPL run in single core, the CPU of this server only run in `2.0 GHz`.
* If `flops_PC = 16`, then `Rpeak = 32 GHz`. 
* `N=24000, Rmax = 29.9 GHz, Efficiency = 93.4%`

```bash
# Summery
-np = 1, cpu_freq = 2.0 GHz, flops_PC = 16,
Rpeak = 32 GHz, 
N=24000, NB=256, P=1, Q=1
Rmax = 29.9 GHz, Efficiency = 93.4%
```



## Server Multi CPU Test

```bash
# Summery
-np = 20, cpu_freq = 1.5 GHz, flops_PC = 16,
Rpeak = 480 GHz, 
N=30000, NB=144, P=4, Q=5
Rmax = 404.4 GHz, Efficiency = 84.2%
```



## Question

* Why CPUs only run in `1.5 GHz` when `-np >= 5` ? BIOS problem, CPU feature, or HPL feature? As its *Base Frequency* is `2.2 GHz`.

* Is `AVX-512` used or not?
* Why `flops_PC = 16` ?

* How to futher optimize it by `CCFLAGS` in `Make.linux`?

    [ICC 19.1: Alphabetical List of Compiler Options](https://software.intel.com/en-us/cpp-compiler-developer-guide-and-reference-alphabetical-list-of-compiler-options)

    * `-march=cascadelake -mtune=cascadelake -mavx -mavx512f -mavx512dq -mavx512cd -mavx512bw -mavx512vl -msse4.2 -msse4.1 -msse3`
        * `Not work`
    * `-fno-omit-frame-pointer -foptimize-sibling-calls -fno-protect-parens -global-hoist -ip -ipo0 -mtune=core-avx2 `
        * `Not work`
    * My first conclusion is comptimizing the HPL by `CCFLAGS` do little works. Since the main performance determinded parts are `blas` and `HPL.dat` .