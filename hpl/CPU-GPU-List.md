#### Xeon E5-2699 v4 (nclab)

```bash
Intel(R) Xeon(R) CPU E5-2699 v4 @ 2.20GHz ×2, $4115.00
22 cores, Broadwell-2016, TDP=145 W
AVX2, SMP=2
		Base	Turbo-1	Turbo-all
Normal	2.3		3.6		2.8
```

https://en.wikichip.org/wiki/intel/xeon_e5/e5-2699_v4



#### Core i7-7700HQ (xps)

```bash
Intel(R) Core(TM) i7-7700HQ CPU @ 2.80GHz ×1, $378.00
4 cores, Kaby Lake-2017, TDP=45 W
AVX2
		Base	Turbo-1	Turbo-all
Normal	2.8		3.8		3.4
```

https://en.wikichip.org/wiki/intel/core_i7/i7-7700hq



#### Xeon Gold 5518 (borrow lc)

```bash
Intel(R) Xeon(R) Gold 5118 CPU @ 2.30GHz ×2, $1273.00
12 cores, Sky Lake-2017, TDP=105 W
AVX-512 (FMA=1), SMP=4 (UPI=3)
		Base	Turbo-1	Turbo-all
Normal	2.3		3.2		2.7
AVX2	1.9		3.1		2.3
AVX512	1.2		2.9		1.6
```

https://en.wikichip.org/wiki/intel/xeon_gold/5118



#### Xeon Silver 4210 (lc)

```bash
Intel(R) Xeon(R) Silver 4210 CPU @ 2.20GHz ×2, $511.00
10 cores, Cascade Lake-2019, TDP=85 W
AVX-512 (FMA=1), SMP=2 (UPI=2)
		Base	Turbo-1	Turbo-all
Normal	2.2		3.2		2.7
AVX2	1.9		3.0		2.3
AVX512	1.2		2.0		1.5
```

https://en.wikichip.org/wiki/intel/xeon_silver/4210



## GPU

https://developer.nvidia.com/hpc-application-performance

https://www.microway.com/knowledge-center-articles/comparison-of-nvidia-geforce-gpus-and-nvidia-tesla-gpus/



```bash
Peak FP64 GFLOPS = FP64 core * Turbo Clock * 2
Peak FP32 GFLOPS = FP32 core * Turbo Clock * 2
	# FP32 core = CUDA core
Peak FP16 GFLOPS = Tensor core * Turbo Clock * 16
Peak Tensor GFLOPS = Tensor core * Turbo Clock * 128
	# Tensor GFLOPS = FP16 Tensor TFLOPS with FP16 Accumulate 
```



#### Tesla V100 (PCIe)

```bash
Name:				Tesla V100-PCIE-32GB
Arch:				Volta

Turbo Clock:		1.380 GHz
FP64 core:			2560
FP32 core:			5120
Tensor core:		640
Peak FP64 TFLOPS:	7 
Peak FP32 TFLOPS:	14 
Peak Tensor TFLOPS:	125

Memory:				32
Memory Bandwidth:	900 GB/sec
Interconnect Bandwidth:	32 GB/sec

TDP:				250 W
Thermal Solution:	Passive
```

[whitepaper: NVIDIA TESLA V100 GPU ARCHITECTURE](https://images.nvidia.com/content/volta-architecture/pdf/volta-architecture-whitepaper.pdf)



#### GeForce RTX 2080 Ti (Reference Edition)

> The TU102 GPU also features 144 FP64 units (two per SM), which are not depicted in this diagram. The FP64 TFLOP rate is 1/32nd the TFLOP rate of FP32 operations. The small number of FP64 hardware units are included to ensure any programs with FP64 code operates correctly.
>
> FP32 core ×32 ==Feature== FP64 core ×1

```bash
Name:				GeForce RTX 2080 Ti
Arch:				Turing

Turbo Clock:		1.545 GHz (2.100?)
FP64 core:			136 (Featured)
FP32 core:			4352
Tensor core:		544
Peak FP64 TFLOPS:	0.42
Peak FP32 TFLOPS:	13.4
Peak Tensor TFLOPS:	107.6

Memory:				11
Memory Bandwidth:	616 GB/sec

TDP:				250 W
Thermal Solution:	Fan
```

[whitepaper: NVIDIA Turing ARCHITECTURE](https://www.nvidia.com/content/dam/en-zz/Solutions/design-visualization/technologies/turing-architecture/NVIDIA-Turing-Architecture-Whitepaper.pdf)



```bash
nvidia-smi -i 0
nvidia-smi -L


```

