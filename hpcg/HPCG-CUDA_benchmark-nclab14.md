#### Testing Environment

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

 `HPCG-3.1_CUDA-10`   `CUDA-10.0.130`  `OpenMPI-3.1.0`

```bash
#!/bin/bash
HPCG_DIR=`pwd`
HPCG_BIN=xhpcg-3.1_gcc_485_cuda-10.0.130_ompi-3.1.0_sm_35_sm_50_sm_60_sm_70_sm_75_ver_10_9_18

# Input
GPU_TYPE="RTX_2080_Ti"
GPU_NUM=8
export CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7
cat > hpcg.dat << EOF
HPCG benchmark input file
Sandia National Laboratories; University of Tennessee, Knoxville
272 272 272
3660
EOF

export OMP_NUM_THREADS=1
MPIFLAGS=" --mca btl_openib_warn_default_gid_prefix 0"

mpirun -np $GPU_NUM $MPIFLAGS $HPCG_DIR/$HPCG_BIN | tee "./results/xhpcg_${GPU_NUM}_gpu-${GPU_TYPE}-$(date +"%G%m%d-%H%M%S").txt"
```



start of application (1 OMP threads)...
2020-01-12 23:18:40.454

Problem setup...
Setup time: 1.312329 sec 

GPU: 'GeForce RTX 2080 Ti' 
Memory use: 9885 MB / 10989 MB
2x2x2 process grid
272x272x272 local domain

Reference SpMV+MG...

Reference CG...
Initial Residual: 1.200981e+04 Max_err: 1.000000e+00 tot_err: 1.268815e+04
REF  Iter = 1 Scaled Residual: 1.866259e-01 Max error: 1.000000e+00 tot_error: 9.858108e-01 
REF  Iter = 2 Scaled Residual: 1.025270e-01 Max error: 1.000000e+00 tot_error: 9.726061e-01 
REF  Iter = 3 Scaled Residual: 7.079028e-02 Max error: 1.000000e+00 tot_error: 9.596411e-01 
REF  Iter = 4 Scaled Residual: 5.408803e-02 Max error: 1.000000e+00 tot_error: 9.467704e-01 
REF  Iter = 5 Scaled Residual: 4.374074e-02 Max error: 1.000000e+00 tot_error: 9.339604e-01 
REF  Iter = 6 Scaled Residual: 3.669711e-02 Max error: 1.000000e+00 tot_error: 9.212045e-01 
REF  Iter = 7 Scaled Residual: 3.160141e-02 Max error: 1.000000e+00 tot_error: 9.084999e-01 
REF  Iter = 8 Scaled Residual: 2.775789e-02 Max error: 1.000000e+00 tot_error: 8.958490e-01 
REF  Iter = 9 Scaled Residual: 2.476621e-02 Max error: 1.000000e+00 tot_error: 8.832536e-01 
REF  Iter = 10 Scaled Residual: 2.237313e-02 Max error: 1.000000e+00 tot_error: 8.707159e-01 
REF  Iter = 11 Scaled Residual: 2.040934e-02 Max error: 1.000000e+00 tot_error: 8.582356e-01 
REF  Iter = 12 Scaled Residual: 1.875848e-02 Max error: 1.000000e+00 tot_error: 8.458118e-01 
REF  Iter = 13 Scaled Residual: 1.734030e-02 Max error: 1.000000e+00 tot_error: 8.334425e-01 
REF  Iter = 14 Scaled Residual: 1.610403e-02 Max error: 1.000000e+00 tot_error: 8.211258e-01 
REF  Iter = 15 Scaled Residual: 1.501881e-02 Max error: 1.000000e+00 tot_error: 8.088616e-01 
REF  Iter = 16 Scaled Residual: 1.406445e-02 Max error: 1.000000e+00 tot_error: 7.966512e-01 
REF  Iter = 17 Scaled Residual: 1.322647e-02 Max error: 1.000000e+00 tot_error: 7.844974e-01 
REF  Iter = 18 Scaled Residual: 1.249101e-02 Max error: 1.000000e+00 tot_error: 7.724032e-01 
REF  Iter = 19 Scaled Residual: 1.184234e-02 Max error: 1.000000e+00 tot_error: 7.603714e-01 
REF  Iter = 20 Scaled Residual: 1.126436e-02 Max error: 1.000000e+00 tot_error: 7.484029e-01 
REF  Iter = 21 Scaled Residual: 1.074142e-02 Max error: 1.000000e+00 tot_error: 7.364975e-01 
REF  Iter = 22 Scaled Residual: 1.026050e-02 Max error: 1.000000e+00 tot_error: 7.246534e-01 
REF  Iter = 23 Scaled Residual: 9.814625e-03 Max error: 1.000000e+00 tot_error: 7.128690e-01 
REF  Iter = 24 Scaled Residual: 9.402140e-03 Max error: 1.000000e+00 tot_error: 7.011437e-01 
REF  Iter = 25 Scaled Residual: 9.024347e-03 Max error: 1.000000e+00 tot_error: 6.894787e-01 
REF  Iter = 26 Scaled Residual: 8.683938e-03 Max error: 1.000000e+00 tot_error: 6.778764e-01 
REF  Iter = 27 Scaled Residual: 8.381710e-03 Max error: 1.000000e+00 tot_error: 6.663409e-01 
REF  Iter = 28 Scaled Residual: 8.114761e-03 Max error: 1.000000e+00 tot_error: 6.548754e-01 
REF  Iter = 29 Scaled Residual: 7.877069e-03 Max error: 1.000000e+00 tot_error: 6.434824e-01 
REF  Iter = 30 Scaled Residual: 7.659820e-03 Max error: 1.000000e+00 tot_error: 6.321625e-01 
REF  Iter = 31 Scaled Residual: 7.454056e-03 Max error: 1.000000e+00 tot_error: 6.209149e-01 
REF  Iter = 32 Scaled Residual: 7.254389e-03 Max error: 1.000000e+00 tot_error: 6.097380e-01 
REF  Iter = 33 Scaled Residual: 7.058981e-03 Max error: 1.000000e+00 tot_error: 5.986315e-01 
REF  Iter = 34 Scaled Residual: 6.868349e-03 Max error: 1.000000e+00 tot_error: 5.875959e-01 
REF  Iter = 35 Scaled Residual: 6.682858e-03 Max error: 1.000000e+00 tot_error: 5.766332e-01 
REF  Iter = 36 Scaled Residual: 6.499643e-03 Max error: 1.000000e+00 tot_error: 5.657449e-01 
REF  Iter = 37 Scaled Residual: 6.313382e-03 Max error: 1.000000e+00 tot_error: 5.549309e-01 
REF  Iter = 38 Scaled Residual: 6.119779e-03 Max error: 1.000000e+00 tot_error: 5.441891e-01 
REF  Iter = 39 Scaled Residual: 5.918435e-03 Max error: 1.000000e+00 tot_error: 5.335185e-01 
REF  Iter = 40 Scaled Residual: 5.715523e-03 Max error: 1.000000e+00 tot_error: 5.229197e-01 
REF  Iter = 41 Scaled Residual: 5.522455e-03 Max error: 1.000000e+00 tot_error: 5.123931e-01 
REF  Iter = 42 Scaled Residual: 5.350284e-03 Max error: 1.000000e+00 tot_error: 5.019326e-01 
REF  Iter = 43 Scaled Residual: 5.202943e-03 Max error: 1.000000e+00 tot_error: 4.915284e-01 
REF  Iter = 44 Scaled Residual: 5.075991e-03 Max error: 1.000000e+00 tot_error: 4.811820e-01 
REF  Iter = 45 Scaled Residual: 4.964571e-03 Max error: 1.000000e+00 tot_error: 4.709036e-01 
REF  Iter = 46 Scaled Residual: 4.864576e-03 Max error: 1.000000e+00 tot_error: 4.606923e-01 
REF  Iter = 47 Scaled Residual: 4.767830e-03 Max error: 1.000000e+00 tot_error: 4.505449e-01 
REF  Iter = 48 Scaled Residual: 4.671028e-03 Max error: 1.000000e+00 tot_error: 4.404626e-01 
REF  Iter = 49 Scaled Residual: 4.573486e-03 Max error: 1.000000e+00 tot_error: 4.304430e-01 
REF  Iter = 50 Scaled Residual: 4.476388e-03 Max error: 1.000000e+00 tot_error: 4.204895e-01 

Optimization...
Optimization time: 3.424249e-01 sec 

Validation...

Optimized CG Setup...
Initial Residual: 1.200981e+04 Max_err: 1.000000e+00 tot_err: 1.268815e+04
Iteration = 1 Scaled Residual: 2.434860e-01 Max error: 1.000000e+00 tot_error: 9.881904e-01 
Iteration = 2 Scaled Residual: 1.343375e-01 Max error: 1.000000e+00 tot_error: 9.769517e-01 
Iteration = 3 Scaled Residual: 9.508341e-02 Max error: 1.000000e+00 tot_error: 9.660937e-01 
Iteration = 4 Scaled Residual: 7.507080e-02 Max error: 1.000000e+00 tot_error: 9.554597e-01 
Iteration = 5 Scaled Residual: 6.270109e-02 Max error: 1.000000e+00 tot_error: 9.450338e-01 
Iteration = 6 Scaled Residual: 5.375866e-02 Max error: 1.000000e+00 tot_error: 9.348704e-01 
Iteration = 7 Scaled Residual: 4.678818e-02 Max error: 1.000000e+00 tot_error: 9.247885e-01 
Iteration = 8 Scaled Residual: 4.064493e-02 Max error: 1.000000e+00 tot_error: 9.146347e-01 
Iteration = 9 Scaled Residual: 3.586849e-02 Max error: 1.000000e+00 tot_error: 9.045078e-01 
Iteration = 10 Scaled Residual: 3.238035e-02 Max error: 1.000000e+00 tot_error: 8.943844e-01 
Iteration = 11 Scaled Residual: 3.010681e-02 Max error: 1.000000e+00 tot_error: 8.842883e-01 
Iteration = 12 Scaled Residual: 2.853984e-02 Max error: 1.000000e+00 tot_error: 8.741977e-01 
Iteration = 13 Scaled Residual: 2.691478e-02 Max error: 1.000000e+00 tot_error: 8.640685e-01 
Iteration = 14 Scaled Residual: 2.487163e-02 Max error: 1.000000e+00 tot_error: 8.539802e-01 
Iteration = 15 Scaled Residual: 2.299548e-02 Max error: 1.000000e+00 tot_error: 8.439789e-01 
Iteration = 16 Scaled Residual: 2.145222e-02 Max error: 1.000000e+00 tot_error: 8.340039e-01 
Iteration = 17 Scaled Residual: 2.024870e-02 Max error: 1.000000e+00 tot_error: 8.242153e-01 
Iteration = 18 Scaled Residual: 1.936433e-02 Max error: 1.000000e+00 tot_error: 8.146017e-01 
Iteration = 19 Scaled Residual: 1.863701e-02 Max error: 1.000000e+00 tot_error: 8.051130e-01 
Iteration = 20 Scaled Residual: 1.787260e-02 Max error: 1.000000e+00 tot_error: 7.955983e-01 
Iteration = 21 Scaled Residual: 1.705340e-02 Max error: 1.000000e+00 tot_error: 7.861028e-01 
Iteration = 22 Scaled Residual: 1.642420e-02 Max error: 1.000000e+00 tot_error: 7.767167e-01 
Iteration = 23 Scaled Residual: 1.597113e-02 Max error: 1.000000e+00 tot_error: 7.673205e-01 
Iteration = 24 Scaled Residual: 1.552416e-02 Max error: 1.000000e+00 tot_error: 7.579348e-01 
Iteration = 25 Scaled Residual: 1.503449e-02 Max error: 1.000000e+00 tot_error: 7.485382e-01 
Iteration = 26 Scaled Residual: 1.446731e-02 Max error: 1.000000e+00 tot_error: 7.392213e-01 
Iteration = 27 Scaled Residual: 1.401125e-02 Max error: 1.000000e+00 tot_error: 7.300646e-01 
Iteration = 28 Scaled Residual: 1.361898e-02 Max error: 1.000000e+00 tot_error: 7.210432e-01 
Iteration = 29 Scaled Residual: 1.319807e-02 Max error: 1.000000e+00 tot_error: 7.120749e-01 
Iteration = 30 Scaled Residual: 1.279213e-02 Max error: 1.000000e+00 tot_error: 7.032861e-01 
Iteration = 31 Scaled Residual: 1.251861e-02 Max error: 1.000000e+00 tot_error: 6.946433e-01 
Iteration = 32 Scaled Residual: 1.228628e-02 Max error: 1.000000e+00 tot_error: 6.860392e-01 
Iteration = 33 Scaled Residual: 1.204442e-02 Max error: 1.000000e+00 tot_error: 6.774524e-01 
Iteration = 34 Scaled Residual: 1.181550e-02 Max error: 1.000000e+00 tot_error: 6.689151e-01 
Iteration = 35 Scaled Residual: 1.161423e-02 Max error: 1.000000e+00 tot_error: 6.605089e-01 
Iteration = 36 Scaled Residual: 1.143549e-02 Max error: 1.000000e+00 tot_error: 6.520945e-01 
Iteration = 37 Scaled Residual: 1.124139e-02 Max error: 1.000000e+00 tot_error: 6.436962e-01 
Iteration = 38 Scaled Residual: 1.104699e-02 Max error: 1.000000e+00 tot_error: 6.354293e-01 
Iteration = 39 Scaled Residual: 1.081370e-02 Max error: 1.000000e+00 tot_error: 6.272685e-01 
Iteration = 40 Scaled Residual: 1.059131e-02 Max error: 1.000000e+00 tot_error: 6.192266e-01 
Iteration = 41 Scaled Residual: 1.044600e-02 Max error: 1.000000e+00 tot_error: 6.113479e-01 
Iteration = 42 Scaled Residual: 1.026102e-02 Max error: 1.000000e+00 tot_error: 6.035290e-01 
Iteration = 43 Scaled Residual: 1.009397e-02 Max error: 1.000000e+00 tot_error: 5.958266e-01 
Iteration = 44 Scaled Residual: 9.972384e-03 Max error: 1.000000e+00 tot_error: 5.882889e-01 
Iteration = 45 Scaled Residual: 9.824380e-03 Max error: 1.000000e+00 tot_error: 5.808201e-01 
Iteration = 46 Scaled Residual: 9.814458e-03 Max error: 1.000000e+00 tot_error: 5.733833e-01 
Iteration = 47 Scaled Residual: 9.669585e-03 Max error: 1.000000e+00 tot_error: 5.660222e-01 
Iteration = 48 Scaled Residual: 9.582407e-03 Max error: 1.000000e+00 tot_error: 5.587179e-01 
Iteration = 49 Scaled Residual: 9.519969e-03 Max error: 1.000000e+00 tot_error: 5.515175e-01 
Iteration = 50 Scaled Residual: 9.388534e-03 Max error: 1.000000e+00 tot_error: 5.443675e-01 
Iteration = 51 Scaled Residual: 9.368129e-03 Max error: 1.000000e+00 tot_error: 5.372885e-01 
Iteration = 52 Scaled Residual: 9.202357e-03 Max error: 1.000000e+00 tot_error: 5.303069e-01 
Iteration = 53 Scaled Residual: 9.110648e-03 Max error: 1.000000e+00 tot_error: 5.234523e-01 
Iteration = 54 Scaled Residual: 9.006890e-03 Max error: 1.000000e+00 tot_error: 5.167474e-01 
Iteration = 55 Scaled Residual: 8.913661e-03 Max error: 1.000000e+00 tot_error: 5.101074e-01 
Iteration = 56 Scaled Residual: 8.839764e-03 Max error: 1.000000e+00 tot_error: 5.036097e-01 
Iteration = 57 Scaled Residual: 8.761927e-03 Max error: 1.000000e+00 tot_error: 4.971740e-01 
Iteration = 58 Scaled Residual: 8.725550e-03 Max error: 1.000000e+00 tot_error: 4.908408e-01 
Iteration = 59 Scaled Residual: 8.605788e-03 Max error: 1.000000e+00 tot_error: 4.846031e-01 
Iteration = 60 Scaled Residual: 8.611686e-03 Max error: 1.000000e+00 tot_error: 4.784710e-01 
Iteration = 61 Scaled Residual: 8.548263e-03 Max error: 1.000000e+00 tot_error: 4.723794e-01 
Iteration = 62 Scaled Residual: 8.566862e-03 Max error: 1.000000e+00 tot_error: 4.663458e-01 
Iteration = 63 Scaled Residual: 8.474186e-03 Max error: 1.000000e+00 tot_error: 4.603743e-01 
Iteration = 64 Scaled Residual: 8.464437e-03 Max error: 1.000000e+00 tot_error: 4.545020e-01 
Iteration = 65 Scaled Residual: 8.395741e-03 Max error: 1.000000e+00 tot_error: 4.486891e-01 
Iteration = 66 Scaled Residual: 8.387724e-03 Max error: 1.000000e+00 tot_error: 4.429592e-01 
Iteration = 67 Scaled Residual: 8.331236e-03 Max error: 1.000000e+00 tot_error: 4.372830e-01 
Iteration = 68 Scaled Residual: 8.282900e-03 Max error: 1.000000e+00 tot_error: 4.317176e-01 
Iteration = 69 Scaled Residual: 8.213561e-03 Max error: 1.000000e+00 tot_error: 4.262330e-01 
Iteration = 70 Scaled Residual: 8.146954e-03 Max error: 1.000000e+00 tot_error: 4.208815e-01 
Iteration = 71 Scaled Residual: 8.127682e-03 Max error: 1.000000e+00 tot_error: 4.155919e-01 
Iteration = 72 Scaled Residual: 8.056043e-03 Max error: 1.000000e+00 tot_error: 4.104117e-01 
Iteration = 73 Scaled Residual: 8.082560e-03 Max error: 1.000000e+00 tot_error: 4.052738e-01 
Iteration = 74 Scaled Residual: 8.013125e-03 Max error: 1.000000e+00 tot_error: 4.002123e-01 
Iteration = 75 Scaled Residual: 8.009259e-03 Max error: 1.000000e+00 tot_error: 3.952301e-01 
Iteration = 76 Scaled Residual: 7.971640e-03 Max error: 1.000000e+00 tot_error: 3.903123e-01 
Iteration = 77 Scaled Residual: 7.951541e-03 Max error: 1.000000e+00 tot_error: 3.854800e-01 
Iteration = 78 Scaled Residual: 7.958170e-03 Max error: 1.000000e+00 tot_error: 3.806866e-01 
Iteration = 79 Scaled Residual: 7.897154e-03 Max error: 1.000000e+00 tot_error: 3.759857e-01 
Iteration = 80 Scaled Residual: 7.941936e-03 Max error: 1.000000e+00 tot_error: 3.713261e-01 
Iteration = 81 Scaled Residual: 7.920942e-03 Max error: 1.000000e+00 tot_error: 3.667051e-01 
Iteration = 82 Scaled Residual: 7.884280e-03 Max error: 1.000000e+00 tot_error: 3.621649e-01 
Iteration = 83 Scaled Residual: 7.890373e-03 Max error: 1.000000e+00 tot_error: 3.576728e-01 
Iteration = 84 Scaled Residual: 7.828011e-03 Max error: 1.000000e+00 tot_error: 3.532678e-01 
Iteration = 85 Scaled Residual: 7.841667e-03 Max error: 9.999999e-01 tot_error: 3.489195e-01 
Iteration = 86 Scaled Residual: 7.845433e-03 Max error: 9.999999e-01 tot_error: 3.446025e-01 
Iteration = 87 Scaled Residual: 7.790653e-03 Max error: 9.999999e-01 tot_error: 3.403622e-01 
Iteration = 88 Scaled Residual: 7.795994e-03 Max error: 9.999998e-01 tot_error: 3.361753e-01 
Iteration = 89 Scaled Residual: 7.750835e-03 Max error: 9.999997e-01 tot_error: 3.320609e-01 
Iteration = 90 Scaled Residual: 7.695107e-03 Max error: 9.999996e-01 tot_error: 3.280420e-01 
Iteration = 91 Scaled Residual: 7.708914e-03 Max error: 9.999995e-01 tot_error: 3.240719e-01 
Iteration = 92 Scaled Residual: 7.671349e-03 Max error: 9.999993e-01 tot_error: 3.201643e-01 
Iteration = 93 Scaled Residual: 7.643010e-03 Max error: 9.999990e-01 tot_error: 3.163265e-01 
Iteration = 94 Scaled Residual: 7.679571e-03 Max error: 9.999986e-01 tot_error: 3.125171e-01 
Iteration = 95 Scaled Residual: 7.660234e-03 Max error: 9.999981e-01 tot_error: 3.087556e-01 
Iteration = 96 Scaled Residual: 7.636705e-03 Max error: 9.999975e-01 tot_error: 3.050537e-01 
Iteration = 97 Scaled Residual: 7.670703e-03 Max error: 9.999966e-01 tot_error: 3.013788e-01 
Iteration = 98 Scaled Residual: 7.658069e-03 Max error: 9.999954e-01 tot_error: 2.977488e-01 
Iteration = 99 Scaled Residual: 7.627250e-03 Max error: 9.999939e-01 tot_error: 2.941809e-01 
Iteration = 100 Scaled Residual: 7.653837e-03 Max error: 9.999919e-01 tot_error: 2.906455e-01 
Iteration = 101 Scaled Residual: 7.653903e-03 Max error: 9.999893e-01 tot_error: 2.871494e-01 
Iteration = 102 Scaled Residual: 7.617683e-03 Max error: 9.999860e-01 tot_error: 2.837144e-01 
Iteration = 103 Scaled Residual: 7.631790e-03 Max error: 9.999817e-01 tot_error: 2.803173e-01 
Iteration = 104 Scaled Residual: 7.646762e-03 Max error: 9.999762e-01 tot_error: 2.769518e-01 
Iteration = 105 Scaled Residual: 7.607242e-03 Max error: 9.999693e-01 tot_error: 2.736451e-01 
Iteration = 106 Scaled Residual: 7.595020e-03 Max error: 9.999605e-01 tot_error: 2.703866e-01 
Iteration = 107 Scaled Residual: 7.626063e-03 Max error: 9.999493e-01 tot_error: 2.671513e-01 
Iteration = 108 Scaled Residual: 7.614397e-03 Max error: 9.999353e-01 tot_error: 2.639552e-01 
Iteration = 109 Scaled Residual: 7.579221e-03 Max error: 9.999178e-01 tot_error: 2.608110e-01 
Iteration = 110 Scaled Residual: 7.594268e-03 Max error: 9.998960e-01 tot_error: 2.576968e-01 
Iteration = 111 Scaled Residual: 7.616435e-03 Max error: 9.998689e-01 tot_error: 2.546072e-01 
Iteration = 112 Scaled Residual: 7.589641e-03 Max error: 9.998356e-01 tot_error: 2.515632e-01 
Iteration = 113 Scaled Residual: 7.569096e-03 Max error: 9.997947e-01 tot_error: 2.485617e-01 
Iteration = 114 Scaled Residual: 7.589818e-03 Max error: 9.997444e-01 tot_error: 2.455842e-01 
Iteration = 115 Scaled Residual: 7.583998e-03 Max error: 9.996832e-01 tot_error: 2.426426e-01 
Iteration = 116 Scaled Residual: 7.525620e-03 Max error: 9.996094e-01 tot_error: 2.397615e-01 
Iteration = 117 Scaled Residual: 7.488600e-03 Max error: 9.995207e-01 tot_error: 2.369336e-01 
Iteration = 118 Scaled Residual: 7.505844e-03 Max error: 9.994135e-01 tot_error: 2.341364e-01 
Iteration = 119 Scaled Residual: 7.518595e-03 Max error: 9.992847e-01 tot_error: 2.313694e-01 
Iteration = 120 Scaled Residual: 7.500445e-03 Max error: 9.991310e-01 tot_error: 2.286403e-01 
Iteration = 121 Scaled Residual: 7.500294e-03 Max error: 9.989474e-01 tot_error: 2.259369e-01 
Iteration = 122 Scaled Residual: 7.536720e-03 Max error: 9.987280e-01 tot_error: 2.232441e-01 
Iteration = 123 Scaled Residual: 7.560076e-03 Max error: 9.984678e-01 tot_error: 2.205690e-01 
Iteration = 124 Scaled Residual: 7.548047e-03 Max error: 9.981619e-01 tot_error: 2.179254e-01 
Iteration = 125 Scaled Residual: 7.545444e-03 Max error: 9.978027e-01 tot_error: 2.153073e-01 
Iteration = 126 Scaled Residual: 7.579481e-03 Max error: 9.973803e-01 tot_error: 2.126999e-01 
Iteration = 127 Scaled Residual: 7.610301e-03 Max error: 9.968864e-01 tot_error: 2.101062e-01 
Iteration = 128 Scaled Residual: 7.602621e-03 Max error: 9.963141e-01 tot_error: 2.075414e-01 
Iteration = 129 Scaled Residual: 7.587221e-03 Max error: 9.956534e-01 tot_error: 2.050063e-01 
Iteration = 130 Scaled Residual: 7.608926e-03 Max error: 9.948890e-01 tot_error: 2.024857e-01 
Iteration = 131 Scaled Residual: 7.654698e-03 Max error: 9.940054e-01 tot_error: 1.999718e-01 
Iteration = 132 Scaled Residual: 7.679113e-03 Max error: 9.929917e-01 tot_error: 1.974726e-01 
Iteration = 133 Scaled Residual: 7.680140e-03 Max error: 9.918352e-01 tot_error: 1.949936e-01 
Iteration = 134 Scaled Residual: 7.702114e-03 Max error: 9.905142e-01 tot_error: 1.925232e-01 
Iteration = 135 Scaled Residual: 7.768120e-03 Max error: 9.890014e-01 tot_error: 1.900441e-01 
Iteration = 136 Scaled Residual: 7.848721e-03 Max error: 9.872736e-01 tot_error: 1.875512e-01 
Iteration = 137 Scaled Residual: 7.911320e-03 Max error: 9.853115e-01 tot_error: 1.850485e-01 
Iteration = 138 Scaled Residual: 7.971255e-03 Max error: 9.830879e-01 tot_error: 1.825312e-01 
Iteration = 139 Scaled Residual: 8.070128e-03 Max error: 9.805593e-01 tot_error: 1.799798e-01 
Iteration = 140 Scaled Residual: 8.217666e-03 Max error: 9.776763e-01 tot_error: 1.773745e-01 
Iteration = 141 Scaled Residual: 8.380415e-03 Max error: 9.743980e-01 tot_error: 1.747085e-01 
Iteration = 142 Scaled Residual: 8.531380e-03 Max error: 9.706883e-01 tot_error: 1.719817e-01 
Iteration = 143 Scaled Residual: 8.688880e-03 Max error: 9.664941e-01 tot_error: 1.691841e-01 
Iteration = 144 Scaled Residual: 8.892633e-03 Max error: 9.617357e-01 tot_error: 1.662914e-01 
Iteration = 145 Scaled Residual: 9.151619e-03 Max error: 9.563241e-01 tot_error: 1.632791e-01 
Iteration = 146 Scaled Residual: 9.432242e-03 Max error: 9.501859e-01 tot_error: 1.601367e-01 
Iteration = 147 Scaled Residual: 9.701738e-03 Max error: 9.432609e-01 tot_error: 1.568628e-01 
Iteration = 148 Scaled Residual: 9.969281e-03 Max error: 9.354710e-01 tot_error: 1.534495e-01 
Iteration = 149 Scaled Residual: 1.027271e-02 Max error: 9.267003e-01 tot_error: 1.498747e-01 
Iteration = 150 Scaled Residual: 1.062881e-02 Max error: 9.168181e-01 tot_error: 1.461142e-01 
Iteration = 151 Scaled Residual: 1.100771e-02 Max error: 9.057268e-01 tot_error: 1.421596e-01 
Iteration = 152 Scaled Residual: 1.136262e-02 Max error: 8.933783e-01 tot_error: 1.380213e-01 
Iteration = 153 Scaled Residual: 1.167845e-02 Max error: 8.797382e-01 tot_error: 1.337133e-01 
Iteration = 154 Scaled Residual: 1.198061e-02 Max error: 8.647447e-01 tot_error: 1.292400e-01 
Iteration = 155 Scaled Residual: 1.229508e-02 Max error: 8.483253e-01 tot_error: 1.246018e-01 
Iteration = 156 Scaled Residual: 1.260577e-02 Max error: 8.304710e-01 tot_error: 1.198167e-01 
Iteration = 157 Scaled Residual: 1.285808e-02 Max error: 8.112965e-01 tot_error: 1.149330e-01 
Iteration = 158 Scaled Residual: 1.300536e-02 Max error: 7.910205e-01 tot_error: 1.100193e-01 
Iteration = 159 Scaled Residual: 1.304588e-02 Max error: 7.698863e-01 tot_error: 1.051426e-01 
Iteration = 160 Scaled Residual: 1.301177e-02 Max error: 7.481146e-01 tot_error: 1.003569e-01 
Iteration = 161 Scaled Residual: 1.292603e-02 Max error: 7.259365e-01 tot_error: 9.571178e-02 
Iteration = 162 Scaled Residual: 1.277480e-02 Max error: 7.036576e-01 tot_error: 9.126503e-02 
Iteration = 163 Scaled Residual: 1.252299e-02 Max error: 6.816624e-01 tot_error: 8.708176e-02 
Iteration = 164 Scaled Residual: 1.215416e-02 Max error: 6.603346e-01 tot_error: 8.321754e-02 
Iteration = 165 Scaled Residual: 1.169167e-02 Max error: 6.399613e-01 tot_error: 7.970208e-02 
Iteration = 166 Scaled Residual: 1.118309e-02 Max error: 6.207035e-01 tot_error: 7.653771e-02 
Iteration = 167 Scaled Residual: 1.066772e-02 Max error: 6.026386e-01 tot_error: 7.371048e-02 
Iteration = 168 Scaled Residual: 1.015766e-02 Max error: 5.858167e-01 tot_error: 7.120150e-02 
Iteration = 169 Scaled Residual: 9.645843e-03 Max error: 5.702758e-01 tot_error: 6.899034e-02 
Iteration = 170 Scaled Residual: 9.128506e-03 Max error: 5.560151e-01 tot_error: 6.705218e-02 
Iteration = 171 Scaled Residual: 8.619434e-03 Max error: 5.429680e-01 tot_error: 6.535528e-02 
Iteration = 172 Scaled Residual: 8.145248e-03 Max error: 5.310065e-01 tot_error: 6.386305e-02 
Iteration = 173 Scaled Residual: 7.729511e-03 Max error: 5.199759e-01 tot_error: 6.253925e-02 
Iteration = 174 Scaled Residual: 7.379980e-03 Max error: 5.097330e-01 tot_error: 6.135249e-02 
Iteration = 175 Scaled Residual: 7.088255e-03 Max error: 5.001638e-01 tot_error: 6.027773e-02 
Iteration = 176 Scaled Residual: 6.840716e-03 Max error: 4.911759e-01 tot_error: 5.929471e-02 
Iteration = 177 Scaled Residual: 6.631040e-03 Max error: 4.826799e-01 tot_error: 5.838535e-02 
Iteration = 178 Scaled Residual: 6.464692e-03 Max error: 4.745757e-01 tot_error: 5.753201e-02 
Iteration = 179 Scaled Residual: 6.353347e-03 Max error: 4.667595e-01 tot_error: 5.671744e-02 
Iteration = 180 Scaled Residual: 6.304470e-03 Max error: 4.591267e-01 tot_error: 5.592603e-02 
Iteration = 181 Scaled Residual: 6.313674e-03 Max error: 4.517030e-01 tot_error: 5.514516e-02 
Iteration = 182 Scaled Residual: 6.365150e-03 Max error: 4.453118e-01 tot_error: 5.436574e-02 
Iteration = 183 Scaled Residual: 6.439633e-03 Max error: 4.399781e-01 tot_error: 5.358167e-02 
Iteration = 184 Scaled Residual: 6.523711e-03 Max error: 4.350616e-01 tot_error: 5.278857e-02 
Iteration = 185 Scaled Residual: 6.613695e-03 Max error: 4.301131e-01 tot_error: 5.198271e-02 
Iteration = 186 Scaled Residual: 6.711830e-03 Max error: 4.250147e-01 tot_error: 5.116099e-02 
Iteration = 187 Scaled Residual: 6.818140e-03 Max error: 4.196593e-01 tot_error: 5.032200e-02 
Iteration = 188 Scaled Residual: 6.923980e-03 Max error: 4.140231e-01 tot_error: 4.946741e-02 
Iteration = 189 Scaled Residual: 7.012279e-03 Max error: 4.081603e-01 tot_error: 4.860268e-02 
Iteration = 190 Scaled Residual: 7.064614e-03 Max error: 4.020707e-01 tot_error: 4.773625e-02 
Iteration = 191 Scaled Residual: 7.070118e-03 Max error: 3.958953e-01 tot_error: 4.687765e-02 
Iteration = 192 Scaled Residual: 7.029971e-03 Max error: 3.897011e-01 tot_error: 4.603543e-02 
Iteration = 193 Scaled Residual: 6.954926e-03 Max error: 3.834774e-01 tot_error: 4.521609e-02 
Iteration = 194 Scaled Residual: 6.858214e-03 Max error: 3.772897e-01 tot_error: 4.442430e-02 
Iteration = 195 Scaled Residual: 6.748712e-03 Max error: 3.712297e-01 tot_error: 4.366364e-02 
Iteration = 196 Scaled Residual: 6.628462e-03 Max error: 3.654555e-01 tot_error: 4.293718e-02 
Iteration = 197 Scaled Residual: 6.495486e-03 Max error: 3.599392e-01 tot_error: 4.224736e-02 
Iteration = 198 Scaled Residual: 6.349310e-03 Max error: 3.546025e-01 tot_error: 4.159524e-02 
Iteration = 199 Scaled Residual: 6.195113e-03 Max error: 3.495997e-01 tot_error: 4.097983e-02 
Iteration = 200 Scaled Residual: 6.043935e-03 Max error: 3.448502e-01 tot_error: 4.039804e-02 
Iteration = 201 Scaled Residual: 5.909193e-03 Max error: 3.406066e-01 tot_error: 3.984518e-02 
Iteration = 202 Scaled Residual: 5.801729e-03 Max error: 3.365721e-01 tot_error: 3.931596e-02 
Iteration = 203 Scaled Residual: 5.725979e-03 Max error: 3.326648e-01 tot_error: 3.880536e-02 
Iteration = 204 Scaled Residual: 5.679164e-03 Max error: 3.288722e-01 tot_error: 3.830922e-02 
Iteration = 205 Scaled Residual: 5.653759e-03 Max error: 3.251641e-01 tot_error: 3.782426e-02 
Iteration = 206 Scaled Residual: 5.641750e-03 Max error: 3.217131e-01 tot_error: 3.734786e-02 
Iteration = 207 Scaled Residual: 5.638263e-03 Max error: 3.188080e-01 tot_error: 3.687764e-02 
Iteration = 208 Scaled Residual: 5.642754e-03 Max error: 3.164645e-01 tot_error: 3.641121e-02 
Iteration = 209 Scaled Residual: 5.657454e-03 Max error: 3.140725e-01 tot_error: 3.594625e-02 
Iteration = 210 Scaled Residual: 5.684172e-03 Max error: 3.115766e-01 tot_error: 3.548083e-02 
Iteration = 211 Scaled Residual: 5.721248e-03 Max error: 3.089508e-01 tot_error: 3.501390e-02 
Iteration = 212 Scaled Residual: 5.762331e-03 Max error: 3.061771e-01 tot_error: 3.454559e-02 
Iteration = 213 Scaled Residual: 5.797748e-03 Max error: 3.032475e-01 tot_error: 3.407726e-02 
Iteration = 214 Scaled Residual: 5.817806e-03 Max error: 3.001631e-01 tot_error: 3.361120e-02 
Iteration = 215 Scaled Residual: 5.816263e-03 Max error: 2.969319e-01 tot_error: 3.314999e-02 
Iteration = 216 Scaled Residual: 5.792240e-03 Max error: 2.935652e-01 tot_error: 3.269599e-02 
Iteration = 217 Scaled Residual: 5.749789e-03 Max error: 2.900756e-01 tot_error: 3.225098e-02 
Iteration = 218 Scaled Residual: 5.695577e-03 Max error: 2.864756e-01 tot_error: 3.181614e-02 
Iteration = 219 Scaled Residual: 5.635945e-03 Max error: 2.827786e-01 tot_error: 3.139226e-02 
Iteration = 220 Scaled Residual: 5.574687e-03 Max error: 2.790014e-01 tot_error: 3.097997e-02 
Iteration = 221 Scaled Residual: 5.512454e-03 Max error: 2.751627e-01 tot_error: 3.057987e-02 
Iteration = 222 Scaled Residual: 5.447880e-03 Max error: 2.712825e-01 tot_error: 3.019249e-02 
Iteration = 223 Scaled Residual: 5.379658e-03 Max error: 2.673813e-01 tot_error: 2.981807e-02 
Iteration = 224 Scaled Residual: 5.308417e-03 Max error: 2.634760e-01 tot_error: 2.945635e-02 
Iteration = 225 Scaled Residual: 5.237485e-03 Max error: 2.595842e-01 tot_error: 2.910645e-02 
Iteration = 226 Scaled Residual: 5.172271e-03 Max error: 2.557041e-01 tot_error: 2.876690e-02 
Iteration = 227 Scaled Residual: 5.118621e-03 Max error: 2.518343e-01 tot_error: 2.843590e-02 
Iteration = 228 Scaled Residual: 5.080863e-03 Max error: 2.479724e-01 tot_error: 2.811156e-02 
Iteration = 229 Scaled Residual: 5.060303e-03 Max error: 2.441171e-01 tot_error: 2.779220e-02 
Iteration = 230 Scaled Residual: 5.054758e-03 Max error: 2.402711e-01 tot_error: 2.747655e-02 
Iteration = 231 Scaled Residual: 5.059321e-03 Max error: 2.364495e-01 tot_error: 2.716381e-02 
Iteration = 232 Scaled Residual: 5.068033e-03 Max error: 2.330306e-01 tot_error: 2.685359e-02 
Iteration = 233 Scaled Residual: 5.075752e-03 Max error: 2.299557e-01 tot_error: 2.654574e-02 
Iteration = 234 Scaled Residual: 5.079479e-03 Max error: 2.269721e-01 tot_error: 2.624022e-02 
Iteration = 235 Scaled Residual: 5.078686e-03 Max error: 2.242640e-01 tot_error: 2.593699e-02 
Iteration = 236 Scaled Residual: 5.074659e-03 Max error: 2.217756e-01 tot_error: 2.563596e-02 
Iteration = 237 Scaled Residual: 5.069218e-03 Max error: 2.193135e-01 tot_error: 2.533713e-02 
Iteration = 238 Scaled Residual: 5.063372e-03 Max error: 2.168173e-01 tot_error: 2.504066e-02 
Iteration = 239 Scaled Residual: 5.056457e-03 Max error: 2.143321e-01 tot_error: 2.474697e-02 
Iteration = 240 Scaled Residual: 5.046101e-03 Max error: 2.118138e-01 tot_error: 2.445680e-02 
Iteration = 241 Scaled Residual: 5.029024e-03 Max error: 2.092691e-01 tot_error: 2.417107e-02 
Iteration = 242 Scaled Residual: 5.002321e-03 Max error: 2.067068e-01 tot_error: 2.389076e-02 
Iteration = 243 Scaled Residual: 4.964688e-03 Max error: 2.041358e-01 tot_error: 2.361673e-02 
Iteration = 244 Scaled Residual: 4.917094e-03 Max error: 2.015949e-01 tot_error: 2.334950e-02 
Iteration = 245 Scaled Residual: 4.862701e-03 Max error: 1.991301e-01 tot_error: 2.308922e-02 
Iteration = 246 Scaled Residual: 4.806085e-03 Max error: 1.966748e-01 tot_error: 2.283567e-02 
Iteration = 247 Scaled Residual: 4.752093e-03 Max error: 1.942276e-01 tot_error: 2.258837e-02 
Iteration = 248 Scaled Residual: 4.704671e-03 Max error: 1.917958e-01 tot_error: 2.234670e-02 
Iteration = 249 Scaled Residual: 4.666020e-03 Max error: 1.894524e-01 tot_error: 2.211002e-02 
Iteration = 250 Scaled Residual: 4.636315e-03 Max error: 1.872653e-01 tot_error: 2.187780e-02 
Iteration = 251 Scaled Residual: 4.614035e-03 Max error: 1.850957e-01 tot_error: 2.164960e-02 
Iteration = 252 Scaled Residual: 4.596752e-03 Max error: 1.829342e-01 tot_error: 2.142508e-02 
Iteration = 253 Scaled Residual: 4.582091e-03 Max error: 1.807942e-01 tot_error: 2.120395e-02 
Iteration = 254 Scaled Residual: 4.568508e-03 Max error: 1.786551e-01 tot_error: 2.098590e-02 
Iteration = 255 Scaled Residual: 4.555670e-03 Max error: 1.765138e-01 tot_error: 2.077056e-02 
Iteration = 256 Scaled Residual: 4.544339e-03 Max error: 1.743920e-01 tot_error: 2.055751e-02 
Iteration = 257 Scaled Residual: 4.535871e-03 Max error: 1.723168e-01 tot_error: 2.034629e-02 
Iteration = 258 Scaled Residual: 4.531498e-03 Max error: 1.706940e-01 tot_error: 2.013650e-02 
Iteration = 259 Scaled Residual: 4.531649e-03 Max error: 1.693372e-01 tot_error: 1.992785e-02 
Iteration = 260 Scaled Residual: 4.535522e-03 Max error: 1.679510e-01 tot_error: 1.972024e-02 
Iteration = 261 Scaled Residual: 4.541055e-03 Max error: 1.665268e-01 tot_error: 1.951377e-02 
Iteration = 262 Scaled Residual: 4.545322e-03 Max error: 1.650581e-01 tot_error: 1.930875e-02 
Iteration = 263 Scaled Residual: 4.545224e-03 Max error: 1.635409e-01 tot_error: 1.910562e-02 
Iteration = 264 Scaled Residual: 4.538257e-03 Max error: 1.619727e-01 tot_error: 1.890487e-02 
Iteration = 265 Scaled Residual: 4.523124e-03 Max error: 1.603523e-01 tot_error: 1.870699e-02 
Iteration = 266 Scaled Residual: 4.499998e-03 Max error: 1.586792e-01 tot_error: 1.851232e-02 
Iteration = 267 Scaled Residual: 4.470419e-03 Max error: 1.569534e-01 tot_error: 1.832108e-02 

Starting Benchmarking Phase... 
Performing 178 CG sets	 expected time: 3660.0 seconds	 expected Perf:     144.1 GF (18.0 GF_per)
2020-01-12 23:25:59.527
progress = 1.1% 	   41.4 / 3660.0 sec elapsed 	 3618.6 sec remain 	   144.022 GF 	 18.003 GF_per
progress = 2.3% 	   82.8 / 3660.0 sec elapsed 	 3577.2 sec remain 	   143.906 GF 	 17.988 GF_per
progress = 3.4% 	  124.1 / 3660.0 sec elapsed 	 3535.9 sec remain 	   143.923 GF 	 17.990 GF_per
progress = 4.5% 	  165.5 / 3660.0 sec elapsed 	 3494.5 sec remain 	   143.922 GF 	 17.990 GF_per
progress = 5.7% 	  206.8 / 3660.0 sec elapsed 	 3453.2 sec remain 	   143.963 GF 	 17.995 GF_per
progress = 6.8% 	  248.2 / 3660.0 sec elapsed 	 3411.8 sec remain 	   143.948 GF 	 17.994 GF_per
progress = 7.9% 	  289.7 / 3660.0 sec elapsed 	 3370.3 sec remain 	   143.919 GF 	 17.990 GF_per
progress = 9.0% 	  331.1 / 3660.0 sec elapsed 	 3328.9 sec remain 	   143.912 GF 	 17.989 GF_per
progress = 10.2% 	  372.5 / 3660.0 sec elapsed 	 3287.5 sec remain 	   143.899 GF 	 17.987 GF_per
progress = 11.3% 	  413.9 / 3660.0 sec elapsed 	 3246.1 sec remain 	   143.879 GF 	 17.985 GF_per
progress = 12.4% 	  455.4 / 3660.0 sec elapsed 	 3204.6 sec remain 	   143.846 GF 	 17.981 GF_per
progress = 13.6% 	  496.9 / 3660.0 sec elapsed 	 3163.1 sec remain 	   143.823 GF 	 17.978 GF_per
progress = 14.7% 	  538.4 / 3660.0 sec elapsed 	 3121.6 sec remain 	   143.803 GF 	 17.975 GF_per
progress = 15.8% 	  579.9 / 3660.0 sec elapsed 	 3080.1 sec remain 	   143.773 GF 	 17.972 GF_per
progress = 17.0% 	  621.4 / 3660.0 sec elapsed 	 3038.6 sec remain 	   143.767 GF 	 17.971 GF_per
progress = 18.1% 	  662.8 / 3660.0 sec elapsed 	 2997.2 sec remain 	   143.757 GF 	 17.970 GF_per
progress = 19.2% 	  704.3 / 3660.0 sec elapsed 	 2955.7 sec remain 	   143.746 GF 	 17.968 GF_per
progress = 20.4% 	  745.8 / 3660.0 sec elapsed 	 2914.2 sec remain 	   143.746 GF 	 17.968 GF_per
progress = 21.5% 	  787.1 / 3660.0 sec elapsed 	 2872.9 sec remain 	   143.766 GF 	 17.971 GF_per
progress = 22.6% 	  828.5 / 3660.0 sec elapsed 	 2831.5 sec remain 	   143.768 GF 	 17.971 GF_per
progress = 23.8% 	  869.9 / 3660.0 sec elapsed 	 2790.1 sec remain 	   143.770 GF 	 17.971 GF_per
progress = 24.9% 	  911.4 / 3660.0 sec elapsed 	 2748.6 sec remain 	   143.765 GF 	 17.971 GF_per
progress = 26.0% 	  952.8 / 3660.0 sec elapsed 	 2707.2 sec remain 	   143.762 GF 	 17.970 GF_per
progress = 27.2% 	  994.2 / 3660.0 sec elapsed 	 2665.8 sec remain 	   143.762 GF 	 17.970 GF_per
progress = 28.3% 	 1035.7 / 3660.0 sec elapsed 	 2624.3 sec remain 	   143.761 GF 	 17.970 GF_per
progress = 29.4% 	 1077.1 / 3660.0 sec elapsed 	 2582.9 sec remain 	   143.762 GF 	 17.970 GF_per
progress = 30.6% 	 1118.6 / 3660.0 sec elapsed 	 2541.4 sec remain 	   143.755 GF 	 17.969 GF_per
progress = 31.7% 	 1160.0 / 3660.0 sec elapsed 	 2500.0 sec remain 	   143.760 GF 	 17.970 GF_per
progress = 32.8% 	 1201.4 / 3660.0 sec elapsed 	 2458.6 sec remain 	   143.763 GF 	 17.970 GF_per
progress = 34.0% 	 1242.8 / 3660.0 sec elapsed 	 2417.2 sec remain 	   143.765 GF 	 17.971 GF_per
progress = 35.1% 	 1284.1 / 3660.0 sec elapsed 	 2375.9 sec remain 	   143.770 GF 	 17.971 GF_per
progress = 36.2% 	 1325.5 / 3660.0 sec elapsed 	 2334.5 sec remain 	   143.773 GF 	 17.972 GF_per
progress = 37.3% 	 1367.0 / 3660.0 sec elapsed 	 2293.0 sec remain 	   143.773 GF 	 17.972 GF_per
progress = 38.5% 	 1408.5 / 3660.0 sec elapsed 	 2251.5 sec remain 	   143.767 GF 	 17.971 GF_per
progress = 39.6% 	 1449.8 / 3660.0 sec elapsed 	 2210.2 sec remain 	   143.779 GF 	 17.972 GF_per
progress = 40.7% 	 1491.1 / 3660.0 sec elapsed 	 2168.9 sec remain 	   143.784 GF 	 17.973 GF_per
progress = 41.9% 	 1532.5 / 3660.0 sec elapsed 	 2127.5 sec remain 	   143.788 GF 	 17.973 GF_per
progress = 43.0% 	 1573.9 / 3660.0 sec elapsed 	 2086.1 sec remain 	   143.787 GF 	 17.973 GF_per
progress = 44.1% 	 1615.4 / 3660.0 sec elapsed 	 2044.6 sec remain 	   143.786 GF 	 17.973 GF_per
progress = 45.3% 	 1656.7 / 3660.0 sec elapsed 	 2003.3 sec remain 	   143.795 GF 	 17.974 GF_per
progress = 46.4% 	 1698.1 / 3660.0 sec elapsed 	 1961.9 sec remain 	   143.798 GF 	 17.975 GF_per
progress = 47.5% 	 1739.5 / 3660.0 sec elapsed 	 1920.5 sec remain 	   143.797 GF 	 17.975 GF_per
progress = 48.7% 	 1780.9 / 3660.0 sec elapsed 	 1879.1 sec remain 	   143.796 GF 	 17.975 GF_per
progress = 49.8% 	 1822.3 / 3660.0 sec elapsed 	 1837.7 sec remain 	   143.798 GF 	 17.975 GF_per
progress = 50.9% 	 1863.7 / 3660.0 sec elapsed 	 1796.3 sec remain 	   143.798 GF 	 17.975 GF_per
progress = 52.1% 	 1905.1 / 3660.0 sec elapsed 	 1754.9 sec remain 	   143.802 GF 	 17.975 GF_per
progress = 53.2% 	 1946.4 / 3660.0 sec elapsed 	 1713.6 sec remain 	   143.809 GF 	 17.976 GF_per
progress = 54.3% 	 1987.7 / 3660.0 sec elapsed 	 1672.3 sec remain 	   143.819 GF 	 17.977 GF_per
progress = 55.4% 	 2029.1 / 3660.0 sec elapsed 	 1630.9 sec remain 	   143.818 GF 	 17.977 GF_per
progress = 56.6% 	 2070.5 / 3660.0 sec elapsed 	 1589.5 sec remain 	   143.820 GF 	 17.977 GF_per
progress = 57.7% 	 2111.8 / 3660.0 sec elapsed 	 1548.2 sec remain 	   143.826 GF 	 17.978 GF_per
progress = 58.8% 	 2153.1 / 3660.0 sec elapsed 	 1506.9 sec remain 	   143.833 GF 	 17.979 GF_per
progress = 60.0% 	 2194.5 / 3660.0 sec elapsed 	 1465.5 sec remain 	   143.834 GF 	 17.979 GF_per
progress = 61.1% 	 2235.8 / 3660.0 sec elapsed 	 1424.2 sec remain 	   143.838 GF 	 17.980 GF_per
progress = 62.2% 	 2277.1 / 3660.0 sec elapsed 	 1382.9 sec remain 	   143.846 GF 	 17.981 GF_per
progress = 63.3% 	 2318.5 / 3660.0 sec elapsed 	 1341.5 sec remain 	   143.847 GF 	 17.981 GF_per
progress = 64.5% 	 2359.9 / 3660.0 sec elapsed 	 1300.1 sec remain 	   143.845 GF 	 17.981 GF_per
progress = 65.6% 	 2401.4 / 3660.0 sec elapsed 	 1258.6 sec remain 	   143.843 GF 	 17.980 GF_per
progress = 66.7% 	 2442.8 / 3660.0 sec elapsed 	 1217.2 sec remain 	   143.840 GF 	 17.980 GF_per
progress = 67.9% 	 2484.3 / 3660.0 sec elapsed 	 1175.7 sec remain 	   143.837 GF 	 17.980 GF_per
progress = 69.0% 	 2525.7 / 3660.0 sec elapsed 	 1134.3 sec remain 	   143.835 GF 	 17.979 GF_per
progress = 70.1% 	 2567.1 / 3660.0 sec elapsed 	 1092.9 sec remain 	   143.838 GF 	 17.980 GF_per
progress = 71.3% 	 2608.4 / 3660.0 sec elapsed 	 1051.6 sec remain 	   143.842 GF 	 17.980 GF_per
progress = 72.4% 	 2649.8 / 3660.0 sec elapsed 	 1010.2 sec remain 	   143.845 GF 	 17.981 GF_per
progress = 73.5% 	 2691.2 / 3660.0 sec elapsed 	  968.8 sec remain 	   143.845 GF 	 17.981 GF_per
progress = 74.7% 	 2732.6 / 3660.0 sec elapsed 	  927.4 sec remain 	   143.842 GF 	 17.980 GF_per
progress = 75.8% 	 2774.0 / 3660.0 sec elapsed 	  886.0 sec remain 	   143.842 GF 	 17.980 GF_per
progress = 76.9% 	 2815.4 / 3660.0 sec elapsed 	  844.6 sec remain 	   143.845 GF 	 17.981 GF_per
progress = 78.1% 	 2856.8 / 3660.0 sec elapsed 	  803.2 sec remain 	   143.846 GF 	 17.981 GF_per
progress = 79.2% 	 2898.2 / 3660.0 sec elapsed 	  761.8 sec remain 	   143.844 GF 	 17.981 GF_per
progress = 80.3% 	 2939.6 / 3660.0 sec elapsed 	  720.4 sec remain 	   143.846 GF 	 17.981 GF_per
progress = 81.4% 	 2981.0 / 3660.0 sec elapsed 	  679.0 sec remain 	   143.844 GF 	 17.981 GF_per
progress = 82.6% 	 3022.4 / 3660.0 sec elapsed 	  637.6 sec remain 	   143.846 GF 	 17.981 GF_per
progress = 83.7% 	 3063.7 / 3660.0 sec elapsed 	  596.3 sec remain 	   143.849 GF 	 17.981 GF_per
progress = 84.8% 	 3105.0 / 3660.0 sec elapsed 	  555.0 sec remain 	   143.855 GF 	 17.982 GF_per
progress = 86.0% 	 3146.3 / 3660.0 sec elapsed 	  513.7 sec remain 	   143.858 GF 	 17.982 GF_per
progress = 87.1% 	 3187.6 / 3660.0 sec elapsed 	  472.4 sec remain 	   143.860 GF 	 17.983 GF_per
progress = 88.2% 	 3229.0 / 3660.0 sec elapsed 	  431.0 sec remain 	   143.862 GF 	 17.983 GF_per
progress = 89.4% 	 3270.2 / 3660.0 sec elapsed 	  389.8 sec remain 	   143.869 GF 	 17.984 GF_per
progress = 90.5% 	 3311.6 / 3660.0 sec elapsed 	  348.4 sec remain 	   143.872 GF 	 17.984 GF_per
progress = 91.6% 	 3353.0 / 3660.0 sec elapsed 	  307.0 sec remain 	   143.871 GF 	 17.984 GF_per
progress = 92.7% 	 3394.3 / 3660.0 sec elapsed 	  265.7 sec remain 	   143.873 GF 	 17.984 GF_per
progress = 93.9% 	 3435.7 / 3660.0 sec elapsed 	  224.3 sec remain 	   143.876 GF 	 17.984 GF_per
progress = 95.0% 	 3477.1 / 3660.0 sec elapsed 	  182.9 sec remain 	   143.875 GF 	 17.984 GF_per
progress = 96.1% 	 3518.5 / 3660.0 sec elapsed 	  141.5 sec remain 	   143.876 GF 	 17.984 GF_per
progress = 97.3% 	 3559.8 / 3660.0 sec elapsed 	  100.2 sec remain 	   143.876 GF 	 17.985 GF_per
progress = 98.4% 	 3601.2 / 3660.0 sec elapsed 	   58.8 sec remain 	   143.878 GF 	 17.985 GF_per
progress = 99.5% 	 3642.5 / 3660.0 sec elapsed 	   17.5 sec remain 	   143.881 GF 	 17.985 GF_per

Completed Benchmarking Phase... elapsed time: 3683.9 seconds 
2020-01-13 00:27:23.461

Number of CG sets:	178 
Iterations per set:	267 
scaled res mean:	4.470419e-03 
scaled res variance:	0.000000e+00 

Total Time: 3.683921e+03 sec 
Setup        Overhead: 0.63%
Optimization Overhead: 0.17%
Convergence  Overhead: 81.27%

2x2x2 process grid
272x272x272 local domain
SpMV  =  607.4 GF (3825.2 GB/s Effective)   75.9 GF_per ( 478.1 GB/s Effective)
SymGS =  839.8 GF (6481.8 GB/s Effective)  105.0 GF_per ( 810.2 GB/s Effective)
total =  774.5 GF (5874.9 GB/s Effective)   96.8 GF_per ( 734.4 GB/s Effective)
final =  143.9 GF (1091.4 GB/s Effective)   18.0 GF_per ( 136.4 GB/s Effective)

end of application...
2020-01-13 00:27:24.059