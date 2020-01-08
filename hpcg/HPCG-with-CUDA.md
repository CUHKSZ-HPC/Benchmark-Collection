## A simple Installation guidance for HPCG with CUDA

#### Installation Environment

```
16.04.6 LTS (Xenial Xerus), 4.4.0-164-generic, x86_64, 315 G Memory
```

```
2 × Intel(R) Xeon(R) CPU E5-2699 v4 @ 2.20GHz
```

```bash
8 × RTX 2080 Ti
lspci
	VGA compatible controller: NVIDIA Corporation Device 1e04 (rev a1)
nvidia-smi -L
	GPU 0: GeForce RTX 2080 Ti
```



#### Download HPCG-3.1_CUDA-10

[hpcg: HPCG Software Releases](https://www.hpcg-benchmark.org/software/index.html)

```bash
wget http://www.hpcg-benchmark.org/downloads/hpcg-3.1_cuda-10_ompi-3.1_gcc485_sm_35_sm_50_sm_60_sm_70_sm75_ver_10_9_18.tgz
tar zxvf hpcg-3.1_cuda-10_ompi-3.1_gcc485_sm_35_sm_50_sm_60_sm_70_sm75_ver_10_9_18.tgz
```

* **NOTICE: `HPCG-3.1_CUDA-10` is a compiled binary, which already dynamically linked to  `CUDA-10.0.130` and `OpenMPI-3.1.0`. So, It doesn't work on other version of `CUDA` and `OpenMPI`.**



#### Install CUDA-10.0.130

[NVIDIA: CUDA Toolkit 10.0 Archive](https://developer.nvidia.com/cuda-10.0-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1804&target_type=deblocal)

```bash
wget https://developer.nvidia.com/compute/cuda/10.0/Prod/local_installers/cuda-repo-ubuntu1804-10-0-local-10.0.130-410.48_1.0-1_amd64
mv cuda-repo-ubuntu1804-10-0-local-10.0.130-410.48_1.0-1_amd64 cuda-repo-ubuntu1804-10-0-local-10.0.130-410.48_1.0-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu1804-10-0-local-10.0.130-410.48_1.0-1_amd64.deb
# The following steps may be unnessary, havn't been test.
sudo apt-key add /var/cuda-repo-10-0-local-10.0.130-410.48/7fa2af80.pub
sudo apt-get update
sudo apt-get install cuda-libraries-10-0
# test
/usr/local/cuda-10.0/bin/nvcc --version
    nvcc: NVIDIA (R) Cuda compiler driver
    Copyright (c) 2005-2018 NVIDIA Corporation
    Built on Sat_Aug_25_21:08:01_CDT_2018
    Cuda compilation tools, release 10.0, V10.0.130
```

* Here I made a harmless mistake: I installed the 18.04 version, but everything worked fine.



#### Install OpenMPI-3.1.0

[open-mpi: Open MPI: Version 3.1](https://www.open-mpi.org/software/ompi/v3.1/)

```bash
wget https://download.open-mpi.org/release/open-mpi/v3.1/openmpi-3.1.0.tar.gz
tar -xzvf openmpi-3.1.0.tar.gz
cd openmpi-3.1.0
mkdir build
cd build
../configure
make -j 44 all
make -j 44 install
export LD_LIBRARY_PATH="/usr/local/lib"
# test
mpirun --version
    mpirun (Open MPI) 3.1.0
    Report bugs to http://www.open-mpi.org/community/help/
```



#### Config HPCG-3.1_CUDA-10

* `cd hpcg-3.1_cuda-10_ompi-3.1_gcc485_sm_35_sm_50_sm_60_sm_70_sm75_ver_10_9_18`

* `vim run`

    * ```bash
        #!/bin/bash
        HPCG_DIR=`pwd`
        HPCG_BIN=xhpcg-3.1_gcc_485_cuda-10.0.130_ompi-3.1.0_sm_35_sm_50_sm_60_sm_70_sm_75_ver_10_9_18
        cp hpcg.dat_256x256x256_60 hpcg.dat 
        
        export OMP_NUM_THREADS=1
        MPIFLAGS=" --mca btl_openib_warn_default_gid_prefix 0"
        
        echo " ****** running HPCG binary=$HPCG_BIN on 8 GPUs ***************************** "
        export CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7
        mpirun -np 8 $MPIFLAGS $HPCG_DIR/$HPCG_BIN | tee ./results/xhpcg_8_gpu.txt
        ```

* `chmod +x run`

* `./run`

    1. It takes about 7 minutes to complete `Reference CG...`, which use CPU.
    2. Then it takes 60 seconds to complete `Starting Benchmarking Phase...`, which use GPU.

    * use `watch -d -n1 nvidia-smi` to see the usage of GPU
    * use `watch -d -n1 nvidia-smi -i 0` to see the usage of single GPU

