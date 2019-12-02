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
    TOPdir       = $(HOME)/Desktop/test/hpl-2.3             # The root directory of hpl
    MPinc        = /usr/include/mpich/                      # dpkg --listfiles libmpich-dev | grep 'mpi\.h'
    MPlib        = /usr/lib/x86_64-linux-gnu/libmpich.so    # dpkg --listfiles libmpich-dev | grep 'libmpich.so'
    LAinc        = /usr/include/x86_64-linux-gnu/atlas      # dpkg --listfiles libatlas-base-dev | grep 'atlas_buildinfo\.h'

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
