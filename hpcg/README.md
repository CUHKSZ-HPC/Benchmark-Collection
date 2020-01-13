# A simple Installation guidance for HPCG(High Performance Conjugate Gradients)

### Related Website:
* HPCG Installation Reference: hpcg-3.1/INSTALL
* [HPCG Official Release](https://www.hpcg-benchmark.org/index.html)
* [HPCG Official Github](https://github.com/hpcg-benchmark/hpcg/)
* The installation of HPCG is exactly the same way as HPL. For people who meet troubles in installing HPCG, you can have a try to install HPL first.
* [top500: HPCG November 2019](https://www.top500.org/hpcg/lists/2019/11/)
* [intel: Intel® Optimized High Performance Conjugate Gradient Benchmark](https://software.intel.com/en-us/mkl-linux-developer-guide-intel-optimized-high-performance-conjugate-gradient-benchmark)
* [paper: HPCG Benchmark:a New Metric for Ranking High Performance Computing Systems](http://www.netlib.org/utk/people/JackDongarra/PAPERS/HPCG-benchmark.pdf)

### Installation Environment:

    Ubuntu 18.04.2 LTS, 4.18.0-15-generic, x86_64, VirtualBox, 2GB Memory, HPCG 3.1

### 0. Install dependency

    sudo apt-get install -y libmpich-dev

### 1. Download HPCG
    cd ~
    wget http://www.hpcg-benchmark.org/downloads/hpcg-3.1.tar.gz
    # unzip the .tar.gz file
    mv hpcg-3.1 hpcg

### 2. Generate HPCG template conffile
```bash
# Working Dir: hpcg-3.1/setup
cp Make.Linux_MPI Make.linux
# These Files will be Generated:
	# hpcg-3.1/Make.linux		# Compilation Configuration File
```

### 3. Modify `Make.linux` according to the text below

    MPdir        = /usr/bin                                 # which mpicc
    MPinc        = -I /usr/include/mpich/                   # dpkg --listfiles libmpich-dev | grep 'mpi\.h'
    MPlib        = /usr/lib/x86_64-linux-gnu/libmpich.so    # dpkg --listfiles libmpich-dev | grep 'libmpich.so'

### 4. Compile HPCG
```bash
# Working Dir: hpcg-3.1/
mkdir build
cd build
../configure linux
make -j $(nproc)
# These Files will be Generated:
	# hpcg-3.1/build/bin/hpcg.dat		# Benchmark Configuration File
	# hpcg-3.1/build/bin/xhpcg			# Executable File
```


### 5. Modify `hpcg.dat` and run the HPCG

##### Modify `hpcg.dat` based on the text below (the original problem size is too large for 2GB memory):

```s
HPCG benchmark input file
Sandia National Laboratories; University of Tennessee, Knoxville
16 16 16
60
```
* `16 16 16` is problem size, limited by system Memory.
* `60` is test time. `60` for optimization, `1860` for official performance test.

##### Execute HPCG program

```bash
# Working Dir: hpcg-3.1/build/bin
mpirun -np 4 ./xhpcg
# These Files will be Generated:
	# hpcg-3.1/build/bin/hpcg20191202T131917.txt
	# hpcg-3.1/build/bin/HPCG-Benchmark_3.1_2019-12-02_13-20-15.txt
```

##### Output Explain

```bash
Final Summary::HPCG result is VALID with a GFLOP/s rating of=13.9936
```



## Also see

* HPCG tests: https://www.cnblogs.com/zhyantao/p/10614243.html 
* MPICH2 Configuration: https://www.cnblogs.com/zhyantao/p/10614237.html



## Miscellaneous

* Statistic of Top 10 super computer

    |                   | Rmax(Tflops) | Rpeak(Tflops) | Efficiency | HPCG(Tflops) | HPCG Efficiency |
    | ----------------- | ------------ | ------------- | ---------- | ------------ | --------------- |
    | Summit            | 148600       | 200794.9      | 74%        | 2925.75      | 1.5%            |
    | Sierra            | 94640        | 125712        | 75%        | 1795.67      | 1.4%            |
    | Sunway TaihuLight | 93014.6      | 125435.9      | 74%        | 480.85       | 0.4%            |
    | Tianhe-2A         | 61444.5      | 100678.7      | 61%        |              |                 |
    | Frontera          | 23516.4      | 38745.9       | 61%        |              |                 |
    | Piz Daint         | 21230        | 27154.3       | 78%        | 496.98       | 1.8%            |
    | Trinity           | 20158.7      | 41461.2       | 49%        | 546.12       | 1.3%            |
    | ABCI              | 19880        | 32576.6       | 61%        | 508.85       | 1.6%            |
    | SuperMUC-NG       | 19476.6      | 26873.9       | 72%        | 207.84       | 0.8%            |
    | Lassen            | 18200        | 23047.2       | 79%        |              |                 |