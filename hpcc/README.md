# A simple Installation guidance for HPCC (HPC Challange Benchmark)

### Related Website:

* [icl.utk: Official Website](https://icl.utk.edu/hpcc/)
* [icl.utk: Download](https://icl.utk.edu/hpcc/software/index.html)
* [ScienceDirect: HPC Benchmark Assessment with Statistical Analysis](https://www.sciencedirect.com/science/article/pii/S1877050914001963)

### Installation Environment:

    Ubuntu 18.04.2 LTS, 4.18.0-15-generic, x86_64, VirtualBox, 2GB Memory, HPCC 1.5.0

### 0. Install dependency

    sudo apt-get install -y libatlas-base-dev mpich libmpich-dev gfortran

### 1. Download HPCC

* Download HPCC package from this website [icl.utk: Download](https://icl.utk.edu/hpcc/software/index.html)

### 2. Generate HPCC template configuration file

```bash
# Working Dir: hpcc-1.5.0/hpl/setup
sh make_generic
cp Make.UNKNOWN ../Make.linux
```

* An configuration file `hpcc-1.5.0/hpl/Make.linux` will be generated.

### 3. Modify `Make.linux` according to the text below

    ARCH         = linux
    MPinc        = /usr/include/mpich/                      # dpkg --listfiles libmpich-dev | grep 'mpi\.h'
    MPlib        = /usr/lib/x86_64-linux-gnu/libmpich.so    # dpkg --listfiles libmpich-dev | grep 'libmpich.so'
    LAinc        = /usr/include/x86_64-linux-gnu/atlas      # dpkg --listfiles libatlas-base-dev | grep 'atlas_buildinfo\.h'

### 4. Compile HPCC

```bash
# Working Dir: hpcc-1.5.0/
make arch=linux -j $(nproc)
```

* An executable file `hpcc-1.5.0/hpcc` will be generated.

### 5. Execute HPCC

##### Fix `cannot open file hpccinf.txt` bug

```bash
# Working Dir: hpcc-1.5.0/
ln -s _hpccinf.txt hpccinf.txt
```

##### Execute HPCC program

```bash
# Working Dir: hpcc-1.5.0/
mpiexec -n 4 ./xhpl
```

* An output file `hpcc-1.5.0/hpccoutf.txt`  will be generated.
