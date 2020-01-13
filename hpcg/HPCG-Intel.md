### 0.Testing Environment:

    Ubuntu 18.04.3 LTS, 
    CentOS Linux 7 (Core)

#### 1.Installation

HPCG-Intel is the component of Intel MKL, one should install Intel® Parallel Studio XE for Linux following the instruction in `AP-Collection/hpl/Intel-MPI-MKL-Installation_Guide.md`

#### 2.Run

```bash
# Working Dir: /opt/intel/mkl/benchmarks/hpcg/bin
export OMP_NUM_THREADS=<N_cores_per_node/N_processes_per_node>
export KMP_AFFINITY=granularity=fine,compact,1,0
# For AVX
mpirun -genvall -n $nprocs -ppn $nprocs_per_node bin/xhpcg_avx  -n$problem_size -t$run_time_in_seconds
# For AVX2
mpirun -genvall -n $nprocs -ppn $nprocs_per_node bin/xhpcg_avx2 -n$problem_size -t$run_time_in_seconds
# For AVX512 on Intel(R) Xeon Phi(TM) processors
mpirun -genvall -n $nprocs -ppn $nprocs_per_node bin/xhpcg_knl  -n$problem_size -t$run_time_in_seconds
# For AVX512 on Intel(R) Xeon(R) Scalable processors
mpirun -genvall -n $nprocs -ppn $nprocs_per_node bin/xhpcg_skx  -n$problem_size -t$run_time_in_seconds
# These Files will be Generated:
	# hpcg_log_2020.01.12.22.21.34.txt
	# n128-20p-1t-V3.0_2020.01.12.22.24.30.yaml
```

#### 2.1 Example:

```bash
# Environment:
Intel(R) Xeon(R) Silver 4210 CPU @ 2.20GHz × 2, 
10 cores, Cascade Lake, 
AVX-512, FMA=1, SMP=2, UPI=2

# Command
mpirun -genvall -n 20 -ppn 20 ./xhpcg_avx2 -n128 -t60

# Output
HPCG result is VALID with a GFLOP/s rating of: 21.8753
# About 55% performance boot compared to regular hpcg
```

