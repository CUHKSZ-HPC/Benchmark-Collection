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
    sudo apt-get install -y libatlas-base-dev libmpich-dev

### 1. Download HPL

    cd ~
    wget https://www.netlib.org/benchmark/hpl/hpl-2.3.tar.gz
    # unzip the .tar.gz file
    mv hpl-2.3 hpl

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
    
    LAdir        = /usr/lib/x86_64-linux-gnu/atlas
    LAinc        = -I /usr/include/x86_64-linux-gnu/ 
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
* To rebuild HPL: `make arch=linux clean_arch_all ; make arch=linux`



### 5. Run the HPL

##### Execute HPL program

```bash
# Working Dir: hpl-2.3/bin/linux
mpiexec -n 4 ./xhpl
# The result is output to stdout
```



### 6.1* Optimization: Multiple Choices of MPI and BLAS

* See `AP-Collection/hpl/Intel-MPI-KML-Installation-guide.md`

* Also see [vitowu: 用 GCC 在单节点上 Build HPL-CPU](http://vitowu.cn/index.php/archives/1326/)




### 6.2* Optimization: HPL Tuning

* `Ns`

    * In order to find out the best performance of your system, the largest problem size fitting in memory is what you should aim for.
    * `Ns² * DOUBLE_SIZE = Memory_SIZE_B`
    * `TARGET_Ns = sqrt( Memory_Size_PER_Node_GB * NUM_Node * 0.8 * 1024³ / 8 ) `

* `NBs`

    * HPL uses the block size NB for the data distribution as well as for the computational granularity.
    * From a data distribution point of view, the smallest NB, the better the load balance. 
    * From a computation point of view, a too small value of NB may limit the computational performance by a large factor because almost no data reuse will occur in the highest level of the memory hierarchy. The number of messages will also increase.
    * Efficient matrix-multiply routines are often internally blocked. Small multiples of this blocking factor are likely to be good block sizes for HPL.
    * The bottom line is that "good" block sizes are almost always in the `32:256:8` interval. The best values depend on the computation / communication performance ratio of your system.

* `Ps, Qs`
* `P×Q = NUM_CORES`
  
* This depends on the physical interconnection network you have. 
  
    * Assuming a mesh or a switch HPL, P and Q should be approximately equal, with Q slightly larger than P: `2×2`
    * If you are running on a simple Ethernet network, there is only one wire through which all the messages are exchanged. On such a network, the performance and scalability of HPL is strongly limited and very flat process grids are likely to be the best choice: `1×4`
    
    

### 7. Miscellaneous

* `Time` output

    * `TOTAL_TIME = Time + residual_check_time`
    * `residual_check_time ≈ Time × 0.12`

* Statistic of Top 10 super computer

    | Name              | Rmax(Tflops) | Rpeak(Tflops) | Efficiency |
    | ----------------- | ------------ | ------------- | ---------- |
    | Summit            | 148600       | 200794.9      | 74%        |
    | Sierra            | 94640        | 125712        | 75%        |
    | Sunway TaihuLight | 93014.6      | 125435.9      | 74%        |
    | Tianhe-2A         | 61444.5      | 100678.7      | 61%        |
    | Frontera          | 23516.4      | 38745.9       | 61%        |
    | Piz Daint         | 21230        | 27154.3       | 78%        |
    | Trinity           | 20158.7      | 41461.2       | 49%        |
    | ABCI              | 19880        | 32576.6       | 61%        |
    | SuperMUC-NG       | 19476.6      | 26873.9       | 72%        |
    | Lassen            | 18200        | 23047.2       | 79%        |



## 7.2 Rpeak

[stack ov: how to calculate hpc performance rpeak [closed]](https://stackoverflow.com/questions/44606021/how-to-calculate-hpc-performance-rpeak)

[drexel: Compiling High Performance Linpack (HPL)](https://proteusmaster.urcf.drexel.edu/urcfwiki/index.php?title=Compiling_High_Performance_Linpack_(HPL)&mobileaction=toggle_view_desktop)

[stack ov: How can CPU's have FLOPS much higher than their clock speeds?](https://stackoverflow.com/questions/51219820/how-can-cpus-have-flops-much-higher-than-their-clock-speeds)

[hlhpc: HPL Tutorials](https://ulhpc-tutorials.readthedocs.io/en/latest/parallel/mpi/HPL/)

[agner: microarchitecture\11.9  #***](https://www.agner.org/optimize/microarchitecture.pdf)

It’s  the theoretical calculated performance of your system.

HPL solves dense linear system in double precision, so the result it gives is `Double Precision Gflops`. **We all use `Double Precision` in calculating float point computation performance**

#### 7.2.1 How to calculate Rpeak

$$
Rpeak=cores×\frac{cycles}{second}×\frac{flops}{cycle}\\
=cores×freq×\frac{flops}{cycle}\\
Unit: Rpeak (Gflops/s), freq(GHz)
$$


$$
Efficiency = \frac{Rmax}{Rpeak}\\
Rmax=Measured, Rpeak=Theoretic
$$

#### 7.2.2 factor - $freq$

$freq$ should be the AVX-512 Turbo Frequency, not the base frequency. Since AVX lower CPU frequency.

* [SIMD instructions lowering CPU frequency](https://stackoverflow.com/questions/56852812/simd-instructions-lowering-cpu-frequency)

* Example: Xeon Silver 4210

    The Base frequency is `2.2 GHz`, Turbo to `3.2 GHz`. But the *All-Core AVX-512 Turbo Frequency* is `1.5 GHz`, use that as the `CPU_freq`.

    ![1578678688267](typora-images-assets/1578678688267.png)
    
* In fact, because of the lowering CPU frequency, often in `1×512-bit FMA Units` case, `AVX2` is much faster than `AVX512`

* [software.intel: AVX512 slower than AVX2 with Intel MKL dgemm on Intel Gold 5118](https://software.intel.com/en-us/forums/software-tuning-performance-optimization-platform-monitoring/topic/815069)

#### 7.2.3 factor - $flops/cycle$

$$
\frac{flops}{cycle}=\frac{operations}{instruction}×\frac{flops}{operation}
$$

​	[wikichip: flops](https://en.wikichip.org/wiki/flops)

* `1×256-bit FMA Units ` carry on **8 $flops/cycle$**
  
    * $\frac{operations}{instruction}=4=vector\_size$
        * $vector\_size=4=\frac{256-bit\ FMA\ Register}{64-bit\ double\ pricision}$
    * $\frac{flops}{operation}=2=FMA\ Feature$
        * The FMA operation has the form *d* = round(*a* · *b* + *c*), so two flops in one operation.
        * [walkingrandomly: Fused Multiply Add (FMA) – One flop or two?](http://www.walkingrandomly.com/?p=4781)
    
*  `1×512-bit FMA Units` carry on **16 $flops/cycle$**

*  `2×256-bit FMA Units` carry on **2*8 $flops/cycle$**

    Modern CPU have at least 2 256-bit FMA Units and can access them at the same time.

    > Haswell microarchitecture is able to handle two FMA instructions at the same time, (on two dedicated execution ports) with a latency of five cycles and a throughput of  one  instruction  per  clock  cycle  and  per  execution  port  (two  FMA  instructions total  per  cycle). [Pawel Gepner: Paper](http://www.cai.sk/ojs/index.php/cai/article/download/2017_5_1001/851)

*  `1×512-bit FMA Units running AVX2` can still carry on **16 $flops/cycle$**

  > This combined 512-bit unit is accessed through port 0, while port 1 can be used for other purposes simultaneously.
  >
  > Floating point vector instructions of 256 bits or less go through port 0 or 1, while 512-bit floating point instructions go through port 0 or 5. [agner: The microarchitecture of Intel CPUs\11.9](https://www.agner.org/optimize/microarchitecture.pdf)
  
  

#### 7.2.4 $flops/cycle$ of CPU flaged `AVX2 & FMA`

| flops/cycle | 2×256-bit FMA | 1×512-bit FMA | 2×512-bit FMA |
| ----------- | ------------- | ------------- | ------------- |
| SSE4_2      | 4             | 4             | 4             |
| AVX         | 8             | 8             | 8             |
| AVX2        | 16            | 16            | 16            |
| AVX-512     |               | 16            | 32            |

* `Skylake` FMA EU

    | Port | EU                     |
    | ---- | ---------------------- |
    | 0    | 256-bit FMA            |
    | 1    | 256-bit FMA            |
    | 5    | 512-bit FMA (optional) |

    * 256-bit FMA of port 0 and port 1 can be logically combined to create the single 512-bit FMA unit when `AVX-512` is supported.

    * > The second AVX-512 unit does not execute AVX-256 instructions

        Or, most AVX-256. There is still some instructions achieved 3 instruction/cycle.

* How to know the number of AVX-512 units?

    * [wikichip: intel](https://en.wikichip.org/wiki/intel)

    

#### Example - Xeon(R) Silver 4210

* `Intel(R) Xeon(R) Silver 4210 CPU @ 2.20GHz, 10 core, Cascade Lake`

    ```bash
  MKL_ENABLE_INSTRUCTIONS=AVX512
  cpu_freq = 2.0 GHz, num_cores = 1, flops/cycle = 16,
  Rpeak = 32 Gflops
  
  cpu_freq = 1.5 GHz, num_cores = 10, flops/cycle = 16,
  Rpeak = 480 Gflops
  ```





## HPL Source Code Analysis

#### Makefile Arch

```bash
# hpl-2.3/Makefile
#    Call makefile in hpl-2.3/Make.top
all              : install
install          : startup refresh build
startup          :
	$(MAKE) -f Make.top startup_dir     arch=$(arch)
	# ...
build            :
	$(MAKE) -f Make.top build_src       arch=$(arch)
	$(MAKE) -f Make.top build_tst       arch=$(arch)

# hpl-2.3/Make.top
#    Read config file in hpl/Make.$(arch), Call makefile in src/*/$(arch) and testing/*/$(arch)
include Make.$(arch)
build_src        :
	( $(CD) src/auxil/$(arch);         $(MAKE) )
	# ...
build_tst        :
	( $(CD) testing/matgen/$(arch);    $(MAKE) )
	# ...
#	 makefile is originally in src/*/Makefile.in, not sure what is `Makefile.in`
	
# hpl-2.3/testing/ptest/linux/Makefile
xhpl             = $(BINdir)/xhpl
HPL_pteobj       = \
   HPL_pddriver.o         HPL_pdinfo.o           HPL_pdtest.o
dexe.grd: $(HPL_pteobj) $(HPLlib)
	$(LINKER) $(LINKFLAGS) -o $(xhpl) $(HPL_pteobj) $(HPL_LIBS)
	$(MAKE) $(BINdir)/HPL.dat
	$(TOUCH) dexe.grd
#	hpl-2.3/testing/ptest/HPL_pddriver.c is where `main()` start
```

#### file call stack

```bash
hpl-2.3/testing/ptest/HPL_pddriver.c # main, read config, prepare env, call next
↓
hpl-2.3/testing/ptest/HPL_pdtest.c # allocate memory, generate matrix, call next, print result, check computation
↓
hpl-2.3/src/pgesv/HPL_pdgesv.c # `factors a N+1-by-N matrix using LU factorization with row partial pivoting`, call next
↓
hpl-2.3/src/pgesv/HPL_pdtrsv.c # core, `solves the upper triangular system of linear equations`, call cblas functions
```

#### How does HPL calculation Gflops?

```bash
# In hpl-2.3/testing/ptest/HPL_pdtest.c:
Gflops = ( ( (double)(N) /   1.0e+9 ) * 
    ( (double)(N) / wtime[0] ) ) * 
    ( ( 2.0 / 3.0 ) * (double)(N) + ( 3.0 / 2.0 ) );
```

#### BLAS function

[netlib: BLAS](http://www.netlib.org/blas/)

According to `hpl-2.3/include/hpl_blas.h`, these are the BLAS functions that HPL used.

```bash
# Level 1
cblas_idamax - index of max abs value
cblas_dswap - swap x and y
cblas_dcopy - copy x into y
cblas_daxpy - y = a*x + y
cblas_dscal - x = a*x
# Level 2
cblas_dgemv - matrix vector multiply
cblas_dger - performs the rank 1 operation A := alpha*x*y' + A
cblas_dtrsv - solving triangular matrix problems
# Level 3
cblas_dgemm - matrix matrix multiply
cblas_dtrsm - solving triangular matrix with multiple right hand sides

# `d = double`

BLAS Level
Level 1 - scalar, vector, vector-vector operation
Level 2 - matrix-vector operation
Level 3 - matrix-matrix operation
```

