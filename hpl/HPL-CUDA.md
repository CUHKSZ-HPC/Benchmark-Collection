[nvidia: NVIDIA Developer Program - ComputeWorks exclusive downloads/CUDA Accelerated Linpack](https://developer.nvidia.com/computeworks-developer-exclusive-downloads)



### Testing Environment:

    Ubuntu 18.04.3 LTS, 4.18.0-15-generic, x86_64, VirtualBox, 2GB Memory, HPL 2.3



### 1. Installation

* Register and download HPL-CUDA from [nvidia: NVIDIA Developer Program - ComputeWorks exclusive downloads/CUDA Accelerated Linpack](https://developer.nvidia.com/computeworks-developer-exclusive-downloads)
* Install Intel® Parallel Studio XE for Linux following the instruction in `AP-Collection/hpl/Intel-MPI-MKL-Installation_Guide.md`



### 2. Config `Make.linuxCUDA`

```bash
# Working File: hpl-2.0_FERMI_v15/Make.linuxCUDA

SHELL        = /bin/sh
CD           = cd
CP           = cp
LN_S         = ln -fs
MKDIR        = mkdir -p
RM           = /bin/rm -f
TOUCH        = touch
ARCH         = linuxCUDA
ifndef  TOPdir
TOPdir = /home/levi/Desktop/ap-collection/test/hpl-2.0_FERMI_v15
endif
INCdir       = $(TOPdir)/include
BINdir       = $(TOPdir)/bin/$(ARCH)
LIBdir       = $(TOPdir)/lib/$(ARCH)
HPLlib       = $(LIBdir)/libhpl.a 

LAdir        = /opt/intel/mkl/lib/intel64
LAinc        =
LAlib        = -L $(TOPdir)/src/cuda  -ldgemm -L/usr/local/cuda/lib64 -lcuda -lcudart -lcublas -L$(LAdir) -lmkl_intel_lp64 -lmkl_intel_thread -lmkl_core -liomp5

F2CDEFS      = -DAdd__ -DF77_INTEGER=int -DStringSunStyle
HPL_INCLUDES = -I$(INCdir) -I$(INCdir)/$(ARCH) $(LAinc) $(MPinc) -I/usr/local/cuda/include

HPL_LIBS     = $(HPLlib) $(LAlib) $(MPlib)
HPL_OPTS     =  -DCUDA
HPL_DEFS     = $(F2CDEFS) $(HPL_OPTS) $(HPL_INCLUDES)

CC      = mpicc
CCFLAGS = $(HPL_DEFS) -fomit-frame-pointer -O3 -funroll-loops -W -Wall -fopenmp
CCNOOPT      = $(HPL_DEFS) -O0 -w
LINKER       = $(CC)
LINKFLAGS    = $(CCFLAGS) 
ARCHIVER     = ar
ARFLAGS      = r
RANLIB       = echo
MAKE = make TOPdir=$(TOPdir)
```



### 3. Make

```bash
# Working Dir: hpl-2.0_FERMI_v15/
make arch=linuxCUDA
```



### 4. Config `run_linpadk`

```bash
# Working File: hpl-2.0_FERMI_v15/bin/linuxCUDA/run_linpack

#!/bin/bash

#location of HPL 
HPL_DIR=/home/levi/Desktop/ap-collection/test/hpl-2.0_FERMI_v15

# Number of CPU cores ( per GPU used = per MPI process )
CPU_CORES_PER_GPU=1
# Number of GPU 
export CUDA_VISIBLE_DEVICES=0,1

# FOR MKL
export MKL_NUM_THREADS=$CPU_CORES_PER_GPU
# FOR GOTO
# export GOTO_NUM_THREADS=$CPU_CORES_PER_GPU
export GOTO_NUM_THREADS=1
# FOR OMP
# export OMP_NUM_THREADS=$CPU_CORES_PER_GPU
export OMP_NUM_THREADS=1

export MKL_DYNAMIC=FALSE

# hint: for 2050 or 2070 card
#       try 350/(350 + MKL_NUM_THREADS*4*cpu frequency in GHz) 
export CUDA_DGEMM_SPLIT=0.80

# hint: try CUDA_DGEMM_SPLIT - 0.10
export CUDA_DTRSM_SPLIT=0.70

export LD_LIBRARY_PATH=$HPL_DIR/src/cuda:$LD_LIBRARY_PATH

# -np should be GPU number
mpirun -np 2 $HPL_DIR/bin/linuxCUDA/xhpl
```



### 5. Config `HPL.dat`

```bash
# Working File: hpl-2.0_FERMI_v15/bin/linuxCUDA/HPL.dat

HPLinpack benchmark input file
Innovative Computing Laboratory, University of Tennessee
HPL.out      output file name (if any)
6            device out (6=stdout,7=stderr,file)
1            # of problems sizes (N)
25000         Ns
1            # of NBs
768          NBs
0            PMAP process mapping (0=Row-,1=Column-major)
1            # of process grids (P x Q)
1            Ps
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
1            DEPTHs (>=0)
1            SWAP (0=bin-exch,1=long,2=mix)
192           swapping threshold
1            L1 in (0=transposed,1=no-transposed) form
1            U  in (0=transposed,1=no-transposed) form
1            Equilibration (0=no,1=yes)
8            memory alignment in double (> 0)
```



### 6. Run

```bash
./run_linpack
```



### 7. Performance

* The performance of `HPL-CUDA` depends on both `CPU_CORES_PER_GPU` and `GPU`. 



### 7. Statistic

```bash
# HPL-CUDA, CPU_CORES_PER_GPU=1, may not be accurate since CPU performance is uncontrol
# The highest performance measured
GeForce RTX 1050 Ti (mobile) = 88 Gflops
GeForce RTX 2080 Ti = 326 Gflops
```

