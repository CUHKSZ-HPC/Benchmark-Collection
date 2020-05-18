This is a GPU enabled binary for the HPCG benchmark.

This binary was built on Centos 7.2 system with gcc 4.8.5, cuda 10.0.130 and openmpi 3.1.

Running requires appropriate CUDA 10 and OpenMPI 3.1.x to be installed on the target system.

***************************
Running the hpcg benchmark
***************************

The implementation assumes a 1:1 mapping between MPI ranks and GPUs and the code automatically maps them.

For example, to run on one node with 2 GPUs :

	mpirun -np 2 ./xhpcg-3.1_gcc_485_cuda-10.0.130_ompi-3.1.0_sm_35_sm_50_sm_60_sm_70_sm_75_ver_10_9_18

To run on two nodes with 4 GPUs each, a command line like below with "hosts2" file being a hostlist with the 2 nodes you are running on.

	mpirun -np 8 -hostfile hosts2 ./xhpcg-3.1_gcc_485_cuda-10.0.130_ompi-3.1.0_sm_35_sm_50_sm_60_sm_70_sm_75_ver_10_9_18

An example running script "run_1_node_2_4_8_gpu_hpcg" is included in the package with several variations for single node run. The paths and parameters can be modified to match your specific system configuration. 


****************************
hpcg.data - input data file
****************************

The benchmark input file is "hpcg.dat". 

This file includes the problem size defined as 3-dimensional grid (ie 256x256x256). 

On  K20x a good local problem size is 256x256x128, on K40/K80/M40/P100/V100-16G  256x256x256 is a good size.

The last line includes a runtime parameter in seconds.

Official submission running times for hpcg should be at least 1 hr (3600) so examples provided use 3660 to be safe.

*****************
Example Results
*****************

A few results summarized below for comparison. The full outputs from some additional runs are included in this package.

************************************
K80, ECC enabled, clk=875
************************************

1 x K80 (2 GPUs)

2x1x1 process grid
256x256x256 local domain
SpMV  =   49.2 GF ( 310.0 GB/s Effective)   24.6 GF_per ( 155.0 GB/s Effective)
SymGS =   68.8 GF ( 531.1 GB/s Effective)   34.4 GF_per ( 265.6 GB/s Effective)
total =   63.4 GF ( 481.1 GB/s Effective)   31.7 GF_per ( 240.6 GB/s Effective)
final =   60.4 GF ( 457.9 GB/s Effective)   30.2 GF_per ( 228.9 GB/s Effective)

2 x K80 (4 GPUs)

2x2x1 process grid
256x256x256 local domain
SpMV  =   98.3 GF ( 618.8 GB/s Effective)   24.6 GF_per ( 154.7 GB/s Effective)
SymGS =  137.1 GF (1058.4 GB/s Effective)   34.3 GF_per ( 264.6 GB/s Effective)
total =  126.5 GF ( 959.5 GB/s Effective)   31.6 GF_per ( 239.9 GB/s Effective)
final =  120.2 GF ( 911.6 GB/s Effective)   30.1 GF_per ( 227.9 GB/s Effective)

4 x K80 (8 GPUs)

2x2x2 process grid
256x256x256 local domain
SpMV  =  196.0 GF (1234.3 GB/s Effective)   24.5 GF_per ( 154.3 GB/s Effective)
SymGS =  272.2 GF (2101.0 GB/s Effective)   34.0 GF_per ( 262.6 GB/s Effective)
total =  251.3 GF (1905.8 GB/s Effective)   31.4 GF_per ( 238.2 GB/s Effective)
final =  227.9 GF (1728.4 GB/s Effective)   28.5 GF_per ( 216.1 GB/s Effective)


************************************
M40 PCIe, ECC enabled, clk=default
************************************

2x1x1 process grid
256x256x256 local domain
SpMV  =   68.9 GF ( 433.7 GB/s Effective)   34.4 GF_per ( 216.8 GB/s Effective)
SymGS =   94.5 GF ( 729.7 GB/s Effective)   47.3 GF_per ( 364.9 GB/s Effective)
total =   87.3 GF ( 662.2 GB/s Effective)   43.7 GF_per ( 331.1 GB/s Effective)
final =   82.9 GF ( 629.0 GB/s Effective)   41.5 GF_per ( 314.5 GB/s Effective)


2x2x1 process grid
256x256x256 local domain
SpMV  =  137.5 GF ( 866.0 GB/s Effective)   34.4 GF_per ( 216.5 GB/s Effective)
SymGS =  188.3 GF (1453.0 GB/s Effective)   47.1 GF_per ( 363.2 GB/s Effective)
total =  174.0 GF (1319.3 GB/s Effective)   43.5 GF_per ( 329.8 GB/s Effective)
final =  165.1 GF (1251.9 GB/s Effective)   41.3 GF_per ( 313.0 GB/s Effective)



************************************
P100 PCIe, ECC enabled, clk=1328
************************************

2 x P100

2x1x1 process grid
256x256x256 local domain
SpMV  =  167.8 GF (1057.0 GB/s Effective)   83.9 GF_per ( 528.5 GB/s Effective)
SymGS =  230.0 GF (1775.1 GB/s Effective)  115.0 GF_per ( 887.5 GB/s Effective)
total =  212.1 GF (1608.3 GB/s Effective)  106.0 GF_per ( 804.1 GB/s Effective)
final =  200.6 GF (1521.2 GB/s Effective)  100.3 GF_per ( 760.6 GB/s Effective)


4 x P100 

2x2x1 process grid
256x256x256 local domain
SpMV  =  333.1 GF (2098.0 GB/s Effective)   83.3 GF_per ( 524.5 GB/s Effective)
SymGS =  455.6 GF (3516.3 GB/s Effective)  113.9 GF_per ( 879.1 GB/s Effective)
total =  419.3 GF (3180.0 GB/s Effective)  104.8 GF_per ( 795.0 GB/s Effective)
final =  396.0 GF (3003.4 GB/s Effective)   99.0 GF_per ( 750.8 GB/s Effective)


************************************
V100-16GB PCIe, ECC enabled, clk=default
************************************

2 x v100

2x1x1 process grid
256x256x256 local domain
SpMV  =  244.4 GF (1539.4 GB/s Effective)  122.2 GF_per ( 769.7 GB/s Effective)
SymGS =  336.7 GF (2598.7 GB/s Effective)  168.3 GF_per (1299.4 GB/s Effective)
total =  309.6 GF (2348.1 GB/s Effective)  154.8 GF_per (1174.0 GB/s Effective)
final =  291.8 GF (2213.2 GB/s Effective)  145.9 GF_per (1106.6 GB/s Effective)

4 x v100

2x2x1 process grid
256x256x256 local domain
SpMV  =  488.4 GF (3075.7 GB/s Effective)  122.1 GF_per ( 768.9 GB/s Effective)
SymGS =  658.8 GF (5084.4 GB/s Effective)  164.7 GF_per (1271.1 GB/s Effective)
total =  608.8 GF (4616.6 GB/s Effective)  152.2 GF_per (1154.1 GB/s Effective)
final =  572.3 GF (4340.0 GB/s Effective)  143.1 GF_per (1085.0 GB/s Effective)






