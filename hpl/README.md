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
make arch=linux -j $(nproc)
# These Files will be Generated:
	# hpl-2.3/bin/linux/HPL.dat		# Benchmark Configuration File
	# hpl-2.3/bin/linux/xhpl		# Executable File
```

### 5. Run the HPL

##### Execute HPL program

```bash
# Working Dir: hpl-2.3/bin/linux
mpiexec -n 4 ./xhpl
# The result is output to stdout
```



#### 6.1* Optimization: Multiple Choices of MPI and BLAS

```bash
# All
ARCH         = linux
TOPdir       = ../../..    # Because all the compilation location are three level higher

# Check Dynamic Linking Status: `ldd bin/linux/xhpl`

# MPI: mpich (apt install libmpich-dev)
MPdir        = 
MPinc        = -I /usr/include/mpich/
MPlib        = /usr/lib/x86_64-linux-gnu/libmpich.so
	# libmpich.so.0 => /usr/lib/x86_64-linux-gnu/libmpich.so.0
# MPI: openmpi (apt install libopenmpi-dev)
MPdir        = /usr/lib/x86_64-linux-gnu/openmpi
MPinc        = -I $(MPdir)/include
MPlib        = $(MPdir)/lib/libmpi.so
	# libmpi.so.20 => /usr/lib/x86_64-linux-gnu/libmpi.so.20
	#              => /usr/lib/x86_64-linux-gnu/libmpi.so.20.10.1
	#              => /usr/lib/x86_64-linux-gnu/openmpi/lib/libmpi.so.20.10.1
	
# BLAS: atlas (apt install libatlas-base-dev)
LAdir        = /usr/lib/x86_64-linux-gnu/atlas
LAinc        = -I /usr/include/x86_64-linux-gnu/ 
LAlib        = -Wl,-rpath=$(LAdir)/ $(LAdir)/libblas.so
	# libblas.so.3 => /usr/lib/x86_64-linux-gnu/atlas/libblas.so.3
# BLAS: blas (*blas-netlib) (apt install libblas-dev)
LAdir        = /usr/lib/x86_64-linux-gnu/blas
LAinc        = -I /usr/include/x86_64-linux-gnu/ 
LAlib        = -Wl,-rpath=$(LAdir)/ $(LAdir)/libblas.so
	# libblas.so.3 => /usr/lib/x86_64-linux-gnu/blas/libblas.so.3
# BLAS: openblas (apt install libopenblas-dev)
LAdir        = /usr/lib/x86_64-linux-gnu/openblas
LAinc        = -I /usr/include/x86_64-linux-gnu/ 
LAlib        = -Wl,-rpath=$(LAdir)/ $(LAdir)/libblas.so
	# libblas.so.3 => /usr/lib/x86_64-linux-gnu/openblas/libblas.so.3
```

