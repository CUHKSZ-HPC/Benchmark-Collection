# A simple Installation guidance for HPL(High Performance Linpack)

### Related Website:
* [HPL Installation Reference](https://www.howtoforge.com/tutorial/hpl-high-performance-linpack-benchmark-raspberry-pi/)
* [HPL Official Release](https://www.netlib.org/benchmark/hpl/)
* [HPL Tuning Basic Idea](http://www.crc.nd.edu/~rich/CRC_Summer_Scholars_2014/HPL-HowTo.pdf)
* [HPL Official Tuning Doc](http://www.netlib.org/benchmark/hpl/tuning.html)
* [HPL Tuning Tool](https://www.advancedclustering.com/act_kb/tune-hpl-dat-file/)
* [HPL Official Q&A](https://www.netlib.org/benchmark/hpl/faqs.html)

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



## 7.2 Rpeak

[stack ov: how to calculate hpc performance rpeak [closed]](https://stackoverflow.com/questions/44606021/how-to-calculate-hpc-performance-rpeak)

[drexel: Compiling High Performance Linpack (HPL)](https://proteusmaster.urcf.drexel.edu/urcfwiki/index.php?title=Compiling_High_Performance_Linpack_(HPL)&mobileaction=toggle_view_desktop)

[stack ov: How can CPU's have FLOPS much higher than their clock speeds?](https://stackoverflow.com/questions/51219820/how-can-cpus-have-flops-much-higher-than-their-clock-speeds)

It’s  the theoretical calculated performance of your system.

HPL solves dense linear system in double precision, so the result it gives is `Gflops in double` (not sure).

#### How to calculate Rpeak

* `Rpeak = CPU_freq × num_cores × flops/cycle`
    * Unit: `Rpeak (Gflops in doube), CPU_freq (GHz)`
* `Efficiency = Rmax / Rpeak`
    * Explain: `Rmax = Measured Performance, Rpeak = Theoretic Performance`

#### factor - CPU_freq

`CPU_freq` should be the actual CPU frequency achieved when running HPL, not the theoretic base frequency. (Not sure)

* For example, `Intel(R) Xeon(R) Silver 4210 CPU @ 2.20GHz, 10 core`. Although its base frequency is `2.2 GHz`, it only achieve `1.5 GHz` when running 8 core on HPL. So, its `CPU_freq_GHz=1.5GHz` when `num_cores=10`.

> Please keep in mind that the recent processors from AMD and Intel utilize frequency scaling. The nominal frequency from the specification might be lower than the maximum frequency, that the processor might be able to use under some circumstances. (https://icl.utk.edu/hpcc/faq/index.html)

> For the theoretical peak, the maximum frequency should be used.

#### factor - flops/cycle

`flops/cycle = fp_instruction/cycle × flops/flops_instruction × vector_size`

* Modern CPU with `FMA` support `16 flops/cycle (double)`

    * `fp_instruction/cycle = 2`

        * > Haswell/Broadwell/Skylake, still 2 floating point operations per cycle.

    * `flops/flops_instruction = 2`
    
        * > A modern CPU such as 8700k can start executing *two* (independent) FMAs in the same cycle.
    
        * The FMA operation has the form *d* = round(*a* · *b* + *c*), so two flops point operation in one instruction.
    
    * `vector_size = 256/64 = 4 (double)`
    
        * `FMA` use 256-bit registers, double is 64 bit
    
        

#### Example - Xeon(R) Silver 4210

* `Intel(R) Xeon(R) Silver 4210 CPU @ 2.20GHz, 10 core, Cascade Lake`

    ```bash
  cpu_freq = 2.0 GHz, num_cores = 1, flops/cycle = 16,
  Rpeak = 32 Gflops
  
  cpu_freq = 1.5 GHz, num_cores = 10, flops/cycle = 16,
  Rpeak = 480 Gflops
  ```
