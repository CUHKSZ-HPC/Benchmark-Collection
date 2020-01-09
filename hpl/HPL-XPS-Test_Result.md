## XPS Single CPU Test (CPU Freq Restrict to 2.5 GHz)

`Intel(R) Core(TM) i7-7700HQ CPU @ 2.80GHz, Kaby Lake`

```bash
HPLinpack benchmark input file
Innovative Computing Laboratory, University of Tennessee
HPL.out      output file name (if any)
6            device out (6=stdout,7=stderr,file)
1            # of problems sizes (N)
10000         Ns
1            # of NBs
256          NBs
0            PMAP process mapping (0=Row-,1=Column-major)
1            # of process grids (P x Q)
1            Ps
1            Qs
16.0         threshold
1            # of panel fact
0            PFACTs (0=left, 1=Crout, 2=Right)
1            # of recursive stopping criterium
2            NBMINs (>= 1)
1            # of panels in recursion
2            NDIVs
1            # of recursive panel fact.
0            RFACTs (0=left, 1=Crout, 2=Right)
1            # of broadcast
0            BCASTs (0=1rg,1=1rM,2=2rg,3=2rM,4=Lng,5=LnM)
1            # of lookahead depth
0            DEPTHs (>=0)
2            SWAP (0=bin-exch,1=long,2=mix)
64           swapping threshold
0            L1 in (0=transposed,1=no-transposed) form
0            U  in (0=transposed,1=no-transposed) form
1            Equilibration (0=no,1=yes)
8            memory alignment in double (> 0)
```



| Gflops      | OpenMPI    | MPICH      | Intel MPI  |
| ----------- | ---------- | ---------- | ---------- |
| netlib-blas |            | 2.1288e+00 |            |
| atlas       |            | 9.4643e+00 |            |
| openblas    |            | 3.4410e+01 | 3.4475e+01 |
| Intel MKL   | 3.6182e+01 | 3.6206e+01 | 3.6211e+01 |

* It seems very little different using `mpirun.mpich` , `mpirun.openmpi`, or `mpirun.intel`
* Also no different to config `alternative-update`

* It may because when using only one CPU, there is no role for MPI to play.

* If `flops_PC = 16`, then `Rpeak = 40 GHz`. This sounds reasonable, since `Rmax = 36 GHz`, it achieve `Efficiency = 90.5%`

* `Rmax` tend to increase as time increase, since CPU needs time to focus.
    * `Ns=12000, Rmax=3.6473e+01`  `Ns=16000, Rmax=3.6893e+01`  `Ns=20000, Rmax=3.7124e+01`  `Ns=24000, Rmax=3.7243e+01`
    * So achieve `Efficiency = 93.1%`

```bash
# Summery
-np = 1, cpu_freq = 2.5 GHz, flops_PC = 16,
Rpeak = 40 GHz, 
N=24000, NB=256, P=1, Q=1
Rmax = 37.243 GHz, Efficiency = 93.1%
```



## Test Script

```bash
#!/bin/bash
cp HPL.dat bin/linux/
cd bin/linux
nice -n -20 taskset --cpu-list 0 mpirun.intel -np 1 ./xhpl
```

```bash
make arch=linux clean_arch_all ; make arch=linux
./run
```

I use `nice` and `taskset`, in order to avoid context switch, so can achieve high-resolution and stable result.

Further solution include: 

1. Interrupt mask to prevent interrupts on certain CPU: [How to give a single process its own CPU core in linux](http://www.hydrogen18.com/blog/howto-give-a-single-process-its-own-cpu-core-in-linux.html)

2. Kernel boot (GRUB) parameter to avoid kernel using certain CPU

3. Close Hyper-Thread

4. Cgroup to isolate CPUs





## Question

* `mpirun -np 1 ./xhpl # without taskset` + `P=1; q=1` will still run ON 8 cores?



