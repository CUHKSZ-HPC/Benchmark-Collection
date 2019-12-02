# A simple Installation guidance for HPCG(High Performance Conjugate Gradients)

### Related Website:
* HPCG Installation Reference: hpcg-3.1/INSTALL
* [HPCG Official Release](https://www.hpcg-benchmark.org/index.html)
* [HPCG Official Github](https://github.com/hpcg-benchmark/hpcg/)
* The installation of HPCG is exactly the same way as HPL. For people who meet troubles in installing HPCG, you can have a try to install HPL first.

### Installation Environment:
    Ubuntu 18.04.2 LTS, 4.18.0-15-generic, x86_64, VirtualBox, 2GB Memory, HPCG 3.1

### 0. Install dependency
    sudo apt-get install -y mpich libmpich-dev

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

	HPCG benchmark input file
	Sandia National Laboratories; University of Tennessee, Knoxville
	16 16 16
	60
##### Execute HPCG program

```bash
# Working Dir: hpcg-3.1/build/bin
mpirun -np 4 ./xhpcg
# These Files will be Generated:
	# hpcg-3.1/build/bin/hpcg20191202T131917.txt
	# hpcg-3.1/build/bin/HPCG-Benchmark_3.1_2019-12-02_13-20-15.txt
```
