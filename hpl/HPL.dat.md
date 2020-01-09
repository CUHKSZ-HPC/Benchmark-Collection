## Origin

```bash
HPLinpack benchmark input file
Innovative Computing Laboratory, University of Tennessee
HPL.out      output file name (if any)
6            device out (6=stdout,7=stderr,file)
4            # of problems sizes (N)
29 30 34 35  Ns
4            # of NBs
1 2 3 4      NBs
0            PMAP process mapping (0=Row-,1=Column-major)
3            # of process grids (P x Q)
2 1 4        Ps
2 4 1        Qs
16.0         threshold
3            # of panel fact
0 1 2        PFACTs (0=left, 1=Crout, 2=Right)
2            # of recursive stopping criterium
2 4          NBMINs (>= 1)
1            # of panels in recursion
2            NDIVs
3            # of recursive panel fact.
0 1 2        RFACTs (0=left, 1=Crout, 2=Right)
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



## XPS

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
2            Ps
2            Qs
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



## XPS Test Result

```bash
# ------Four core test------
# netlibblas + mpich
WR00L2L2       10000   256     1     4             108.32             6.1558e+00
# atlas + mpich
WR00L2L2       10000   256     1     4              21.29             3.1317e+01
# openblas + openmpi + taskset
WR00L2L2       10000   256     1     4              12.73             5.2399e+01
# openblas + mpich + taskset
WR00L2L2       10000   256     1     4              11.95             5.5802e+01

# -----One core test-----
# openblas + mpich + taskset
WR00L2L2       10000   256     1     1              19.57             3.4079e+01
# MKL + mpich + taskset
WR00L2L2       10000   256     1     1              36.23             1.8405e+01
# MKL + IntelMPI + taskset
WR00L2L2       10000   256     1     1              18.82             3.5425e+01
```

```bash
# ./run
#!/bin/bash
cp HPL.dat bin/linux/
cd bin/linux
taskset --cpu-list 0 mpirun -np 1 ./xhpl
```

```bash
make arch=linux clean_arch_all ; make arch=linux
./run
```



## Question

* `mpirun -np 1 ./xhpl` without taskset will still run 8 cores? And acchieve much higher result?