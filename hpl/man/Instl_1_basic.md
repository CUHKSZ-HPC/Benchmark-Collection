# A simple Installation guidance for HPL(High Performance Linpack)

### Related Website:

* [HPL Installation Reference](https://www.howtoforge.com/tutorial/hpl-high-performance-linpack-benchmark-raspberry-pi/)
    * [HPL Official Release](https://www.netlib.org/benchmark/hpl/)
* [HPL Tuning Basic Idea](http://www.crc.nd.edu/~rich/CRC_Summer_Scholars_2014/HPL-HowTo.pdf)
* [HPL Official Tuning Doc](http://www.netlib.org/benchmark/hpl/tuning.html)
* [HPL Tuning Tool](https://www.advancedclustering.com/act_kb/tune-hpl-dat-file/)
* [HPL Official Q&A](https://www.netlib.org/benchmark/hpl/faqs.html)
* [top500: HPL November 2019](https://www.top500.org/lists/2019/11/)

### Installation Environment:

    Ubuntu 18.04.2 LTS, 4.18.0-15-generic, x86_64, VirtualBox, 2GB Memory, HPL 2.3

### 0. Install dependency

    sudo apt-get install -y libmpich-dev libopenblas-dev

### 1. Download HPL

    cd ~
    wget https://www.netlib.org/benchmark/hpl/hpl-2.3.tar.gz
    tar zxvf hpl-2.3.tar.gz

### 2. Generate HPL template conffile

```bash
# Working Dir: hpl-2.3/setup
sh make_generic
cp Make.UNKNOWN ../Make.linux
# These Files will be Generated:
	# hpl-2.3/Make.linux		# Compilation Configuration File
```

### 3. Modify `Make.linux` according to the text below

    ARCH         = linux
    TOPdir       = ../../..
    
    MPdir        = 
    MPinc        = -I /usr/include/mpich/
    MPlib        = /usr/lib/x86_64-linux-gnu/libmpich.so
    
    LAdir        = /usr/lib/x86_64-linux-gnu/openblas
    LAinc        = 
    LAlib        = -Wl,-rpath=$(LAdir)/ $(LAdir)/libblas.so

### 4. Compile HPL

```bash
# Working Dir: hpl-2.3/
make arch=linux
# These Files will be Generated:
	# hpl-2.3/bin/linux/HPL.dat		# Benchmark Configuration File
	# hpl-2.3/bin/linux/xhpl		# Executable File
```

* **Notice: Do not use ` -j $(nproc)` ! Apparently HPL is not smart enough to write a thread-safe Makefile.**

* To rebuild HPL: 

    * ```bash
        make arch=linux clean_arch_all ; make arch=linux
        ```



### 5. Run the HPL

##### Run HPL program on default config

```bash
# Working Dir: hpl-2.3/bin/linux
mpirun -n 4 ./xhpl
# The result is output to stdout
```

##### Run HPL program on single cpu

```bash
# File: hpl-2.3/bin/linux/HPL.dat
```

```bash
HPLinpack benchmark input file
Innovative Computing Laboratory, University of Tennessee
HPL.out      output file name (if any)
6            device out (6=stdout,7=stderr,file)
1            # of problems sizes (N)
8000        Ns
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

```bash
nice -n 10 taskset --cpu-list 0 mpirun -n 1 ./xhpl
# or 
CPU_CORES_PER_GPU=1
export MKL_NUM_THREADS=$CPU_CORES_PER_GPU
export GOTO_NUM_THREADS=$CPU_CORES_PER_GPU
export OMP_NUM_THREADS=$CPU_CORES_PER_GPU
mpirun -n 1 ./xhpl
```
