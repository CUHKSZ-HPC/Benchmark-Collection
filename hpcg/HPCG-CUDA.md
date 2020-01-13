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



#### 1. Download HPCG-3.1_CUDA-10

[hpcg: HPCG Software Releases](https://www.hpcg-benchmark.org/software/index.html)

```bash
wget http://www.hpcg-benchmark.org/downloads/hpcg-3.1_cuda-10_ompi-3.1_gcc485_sm_35_sm_50_sm_60_sm_70_sm75_ver_10_9_18.tgz
tar zxvf hpcg-3.1_cuda-10_ompi-3.1_gcc485_sm_35_sm_50_sm_60_sm_70_sm75_ver_10_9_18.tgz
```

* **NOTICE: `HPCG-3.1_CUDA-10` is a compiled binary, which already dynamically linked to  `CUDA-10.0.130` and `OpenMPI-3.1.0`. So, It doesn't work on other version of `CUDA` and `OpenMPI`.**



#### 2. Install CUDA-10.0.130

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



#### 3. Install OpenMPI-3.1.0

[open-mpi: Open MPI: Version 3.1](https://www.open-mpi.org/software/ompi/v3.1/)

```bash
wget https://download.open-mpi.org/release/open-mpi/v3.1/openmpi-3.1.0.tar.gz
tar -xzvf openmpi-3.1.0.tar.gz
cd openmpi-3.1.0
mkdir build
cd build
../configure
make -j 44 all
sudo make -j 44 install
export LD_LIBRARY_PATH="/usr/local/lib"
# test
mpirun --version
    mpirun (Open MPI) 3.1.0
    Report bugs to http://www.open-mpi.org/community/help/
```



#### 4. Config HPCG-3.1_CUDA-10

* `cd hpcg-3.1_cuda-10_ompi-3.1_gcc485_sm_35_sm_50_sm_60_sm_70_sm75_ver_10_9_18`

* `vim run`

    * ```bash
        #!/bin/bash
        HPCG_DIR=`pwd`
        HPCG_BIN=xhpcg-3.1_gcc_485_cuda-10.0.130_ompi-3.1.0_sm_35_sm_50_sm_60_sm_70_sm_75_ver_10_9_18
        
        # Input
        GPU_TYPE="RTX_2080_Ti"
        GPU_NUM=4
        export CUDA_VISIBLE_DEVICES=0,1,2,3
        cat > hpcg.dat << EOF
        HPCG benchmark input file
        Sandia National Laboratories; University of Tennessee, Knoxville
        128 128 128
        60
        EOF
        
        export OMP_NUM_THREADS=1
        MPIFLAGS=" --mca btl_openib_warn_default_gid_prefix 0"
        
        mpirun -np $GPU_NUM $MPIFLAGS $HPCG_DIR/$HPCG_BIN | tee "./results/xhpcg_${GPU_NUM}_gpu-${GPU_TYPE}-$(date +"%G%m%d-%H%M%S").txt"
        ```

* `chmod +x run`

* `./run`

    1. It takes about 7 minutes to complete `Reference CG...`, which use CPU.
    2. Then it takes 60 seconds to complete `Starting Benchmarking Phase...`, which use GPU.

    * use `watch -d -n1 nvidia-smi` to see the usage of GPU
    * use `watch -d -n1 nvidia-smi -i 0` to see the usage of single GPU



#### Input Explain

* `128 128 128` is problem size, limited by GPU Memory.

    * |             | Memory (MiB) |
        | ----------- | ------------ |
        | 128 128 128 | 1163         |
        | 192 192 192 | 3525         |
        | 224 224 224 | 5151         |
        | 256 256 256 | 8293         |
        | 272 272 272 | 9265         |

* `60` is test time. `60` for optimization, `3660` for official performance test.



#### Output Explain

* ```bash
    SpMV  =  324.7 GF (2045.5 GB/s Effective)   81.2 GF_per ( 511.4 GB/s Effective)
    SymGS =  405.6 GF (3131.2 GB/s Effective)  101.4 GF_per ( 782.8 GB/s Effective)
    ```

    * [hpcg: Performance Overview](https://www.hpcg-benchmark.org/custom/index.html?lid=158&slid=281)

* ```bash
    total =  810.5 GF (6146.1 GB/s Effective)  101.3 GF_per ( 768.3 GB/s Effective)
    ```

    * `810.5 GF` is the speed of all GPUs combined.
    * `101.3 GF_per` is the speed of single GPU.



#### Statistic

```bash
# For HPCG-CUDA, total
# The highest performance measured
Tesla K40 = 34.7 Gflops 	# From HPCG-CUDA
Tesla M40 = 43.7 Gflops	# From HPCG-CUDA
GeFore RTX 2080 Ti = 101.3 Gflops
Tesla P100 = 105.8 Gflops	# From HPCG-CUDA
Tesla V100 = 154.9 Gflops	# From HPCG-CUDA
```

