# A simple Installation guidance for HPCG(High Performance Conjugate Gradients)

### Related Website:
* HPCG Installation Reference: hpcg-3.1/INSTALL
* [HPCG Official Release](https://www.hpcg-benchmark.org/index.html)
* [HPCG Official Github](https://github.com/hpcg-benchmark/hpcg/)
* The installation of HPCG is exactly the same way as HPL. For people who meet troubles in installing HPCG, you can have a try to install HPL first.

### Installation Environment:
    Ubuntu 18.04.2 LTS, 4.18.0-15-generic, x86_64, VirtualBox, 2GB Memory

### 0. Install dependency
    sudo apt-get install -y mpich libmpich-dev

### 1. Download HPCG
    cd ~
    wget http://www.hpcg-benchmark.org/downloads/hpcg-3.1.tar.gz
    # unzip the .tar.gz file
    mv hpcg-3.1 hpcg

### 2. Generate HPCG template conffile
    cd hpcg/setup
    cp Make.Linux_MPI Make.linux

### 3. Modify Make.linux according to the text below
    MPdir        = /usr/bin                                 # which mpicc
    MPinc        = -I /usr/include/mpich/                   # dpkg --listfiles libmpich-dev | grep 'mpi\.h'
    MPlib        = /usr/lib/x86_64-linux-gnu/libmpich.so    # dpkg --listfiles libmpich-dev | grep 'libmpich.so'

### 4. Compile HPCG
    cd ..
    mkdir build
    cd build
    ../configure linux
    make -j $(nproc)

### 5. Modify hpcg.dat and run the HPCG
    cd bin

##### Replace hpcg.dat by the text below:
	HPCG benchmark input file
	Sandia National Laboratories; University of Tennessee, Knoxville
	16 16 16
	60
##### Execute HPCG program
    ./xhpcg

##### Execute HPCG program on multiple node
    mpirun -np 8 ./xhpcg

##### The output will be a file name 'HPCG-Benchmark_3.1_2019-10-15_20-15-01.txt' generated under same directory with content like:
	HPCG-Benchmark
	version=3.1
	Release date=March 28, 2019
	Machine Summary=
	Machine Summary::Distributed Processes=1
	Machine Summary::Threads per processes=1
	Global Problem Dimensions=
	Global Problem Dimensions::Global nx=16
	Global Problem Dimensions::Global ny=16
	Global Problem Dimensions::Global nz=16
	Processor Dimensions=
	Processor Dimensions::npx=1
	Processor Dimensions::npy=1
	Processor Dimensions::npz=1
	Local Domain Dimensions=
	Local Domain Dimensions::nx=16
	Local Domain Dimensions::ny=16
	Local Domain Dimensions::Lower ipz=0
	Local Domain Dimensions::Upper ipz=0
	Local Domain Dimensions::nz=16
	########## Problem Summary  ##########=
	Setup Information=
	Setup Information::Setup Time=0.0205922
	Linear System Information=
	Linear System Information::Number of Equations=4096
	Linear System Information::Number of Nonzero Terms=97336
	Multigrid Information=
	Multigrid Information::Number of coarse grid levels=3
	Multigrid Information::Coarse Grids=
	Multigrid Information::Coarse Grids::Grid Level=1
	Multigrid Information::Coarse Grids::Number of Equations=512
	Multigrid Information::Coarse Grids::Number of Nonzero Terms=10648
	Multigrid Information::Coarse Grids::Number of Presmoother Steps=1
	Multigrid Information::Coarse Grids::Number of Postsmoother Steps=1
	Multigrid Information::Coarse Grids::Grid Level=2
	Multigrid Information::Coarse Grids::Number of Equations=64
	Multigrid Information::Coarse Grids::Number of Nonzero Terms=1000
	Multigrid Information::Coarse Grids::Number of Presmoother Steps=1
	Multigrid Information::Coarse Grids::Number of Postsmoother Steps=1
	Multigrid Information::Coarse Grids::Grid Level=3
	Multigrid Information::Coarse Grids::Number of Equations=8
	Multigrid Information::Coarse Grids::Number of Nonzero Terms=64
	Multigrid Information::Coarse Grids::Number of Presmoother Steps=1
	Multigrid Information::Coarse Grids::Number of Postsmoother Steps=1
	########## Memory Use Summary  ##########=
	Memory Use Information=
	Memory Use Information::Total memory used for data (Gbytes)=0.00293857
	Memory Use Information::Memory used for OptimizeProblem data (Gbytes)=0
	Memory Use Information::Bytes per equation (Total memory / Number of Equations)=717.424
	Memory Use Information::Memory used for linear system and CG (Gbytes)=0.00258607
	Memory Use Information::Coarse Grids=
	Memory Use Information::Coarse Grids::Grid Level=1
	Memory Use Information::Coarse Grids::Memory used=0.000308216
	Memory Use Information::Coarse Grids::Grid Level=2
	Memory Use Information::Coarse Grids::Memory used=3.8968e-05
	Memory Use Information::Coarse Grids::Grid Level=3
	Memory Use Information::Coarse Grids::Memory used=5.312e-06
	########## V&V Testing Summary  ##########=
	Spectral Convergence Tests=
	Spectral Convergence Tests::Result=PASSED
	Spectral Convergence Tests::Unpreconditioned=
	Spectral Convergence Tests::Unpreconditioned::Maximum iteration count=11
	Spectral Convergence Tests::Unpreconditioned::Expected iteration count=12
	Spectral Convergence Tests::Preconditioned=
	Spectral Convergence Tests::Preconditioned::Maximum iteration count=1
	Spectral Convergence Tests::Preconditioned::Expected iteration count=2
	Departure from Symmetry |x'Ay-y'Ax|/(2*||x||*||A||*||y||)/epsilon=
	Departure from Symmetry |x'Ay-y'Ax|/(2*||x||*||A||*||y||)/epsilon::Result=PASSED
	Departure from Symmetry |x'Ay-y'Ax|/(2*||x||*||A||*||y||)/epsilon::Departure for SpMV=6.29632e-05
	Departure from Symmetry |x'Ay-y'Ax|/(2*||x||*||A||*||y||)/epsilon::Departure for MG=3.71658e-06
	########## Iterations Summary  ##########=
	Iteration Count Information=
	Iteration Count Information::Result=PASSED
	Iteration Count Information::Reference CG iterations per set=50
	Iteration Count Information::Optimized CG iterations per set=50
	Iteration Count Information::Total number of reference iterations=59750
	Iteration Count Information::Total number of optimized iterations=59750
	########## Reproducibility Summary  ##########=
	Reproducibility Information=
	Reproducibility Information::Result=PASSED
	Reproducibility Information::Scaled residual mean=3.66234e-41
	Reproducibility Information::Scaled residual variance=0
	########## Performance Summary (times in sec) ##########=
	Benchmark Time Summary=
	Benchmark Time Summary::Optimization phase=2.38419e-07
	Benchmark Time Summary::DDOT=1.30756
	Benchmark Time Summary::WAXPBY=0.699755
	Benchmark Time Summary::SpMV=7.1738
	Benchmark Time Summary::MG=47.1471
	Benchmark Time Summary::Total=56.3567
	Floating Point Operations Summary=
	Floating Point Operations Summary::Raw DDOT=1.47821e+09
	Floating Point Operations Summary::Raw WAXPBY=1.47821e+09
	Floating Point Operations Summary::Raw SpMV=1.18643e+10
	Floating Point Operations Summary::Raw MG=6.51332e+10
	Floating Point Operations Summary::Total=7.99539e+10
	Floating Point Operations Summary::Total with convergence overhead=7.99539e+10
	GB/s Summary=
	GB/s Summary::Raw Read B/W=8.77158
	GB/s Summary::Raw Write B/W=2.02881
	GB/s Summary::Raw Total B/W=10.8004
	GB/s Summary::Total with convergence and optimization phase overhead=10.3485
	GFLOP/s Summary=
	GFLOP/s Summary::Raw DDOT=1.1305
	GFLOP/s Summary::Raw WAXPBY=2.11246
	GFLOP/s Summary::Raw SpMV=1.65384
	GFLOP/s Summary::Raw MG=1.38149
	GFLOP/s Summary::Raw Total=1.41871
	GFLOP/s Summary::Total with convergence overhead=1.41871
	GFLOP/s Summary::Total with convergence and optimization phase overhead=1.35936
	User Optimization Overheads=
	User Optimization Overheads::Optimization phase time (sec)=2.38419e-07
	User Optimization Overheads::Optimization phase time vs reference SpMV+MG time=0.000260879
	DDOT Timing Variations=
	DDOT Timing Variations::Min DDOT MPI_Allreduce time=0.0760264
	DDOT Timing Variations::Max DDOT MPI_Allreduce time=0.0760264
	DDOT Timing Variations::Avg DDOT MPI_Allreduce time=0.0760264
	Final Summary=
	Final Summary::HPCG result is VALID with a GFLOP/s rating of=1.35936
	Final Summary::HPCG 2.4 rating for historical reasons is=1.41871
	Final Summary::Reference version of ComputeDotProduct used=Performance results are most likely suboptimal
	Final Summary::Reference version of ComputeSPMV used=Performance results are most likely suboptimal
	Final Summary::Reference version of ComputeMG used=Performance results are most likely suboptimal
	Final Summary::Reference version of ComputeWAXPBY used=Performance results are most likely suboptimal
	Final Summary::Results are valid but execution time (sec) is=56.3567
	Final Summary::Official results execution time (sec) must be at least=1800	
