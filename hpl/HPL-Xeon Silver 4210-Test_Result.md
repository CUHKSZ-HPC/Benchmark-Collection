## CPU Info

```bash
Intel(R) Xeon(R) Silver 4210 CPU @ 2.20GHz × 2, 
10 cores, Cascade Lake, 
AVX-512, FMA=1, SMP=2, UPI=2
```

|        | 1 core turbo freq | 20 core turbo freq |
| ------ | ----------------- | ------------------ |
| SSE4_2 | 3.2               | 2.7                |
| AVX    | 3                 | 2.3                |
| AVX2   | 3                 | 2.3                |
| AVX512 | 2                 | 1.5                |



## Single Core Test

```bash
# Summery
MKL_ENABLE_INSTRUCTIONS=AVX512
-np = 1, cpu_freq = 2.0 GHz, flops_PC = 16,
Rpeak = 32 GHz, 
N=24000, NB=256, P=1, Q=1
Rmax = 29.9 GHz, Efficiency = 93.4%
```



## Multi Core Test

```bash
# Summery
MKL_ENABLE_INSTRUCTIONS=AVX512
-np = 20, cpu_freq = 1.5 GHz, flops_PC = 16,
Rpeak = 480 GHz, 
N=30000, NB=144, P=4, Q=5
Rmax = 404.4 GHz, Efficiency = 84.2%
```

```bash
# Summery
MKL_ENABLE_INSTRUCTIONS=AVX2
-np = 20, cpu_freq = 2.3 GHz, flops_PC = 16,
Rpeak = 736 GHz, 
N=80000, NB=144, P=4, Q=5
Rmax = 633.6 GHz, Efficiency = 86.1%
```







## Question

* Why CPUs only run in `1.5 GHz` when `-np >= 5` ? BIOS problem, CPU feature, or HPL feature? As its *Base Frequency* is `2.2 GHz`.

    * How about `Turn off CPU throttling`
      
        * `Not work, tried /sys/devices/system/cpu/intel_pstate and /sys/devices/system/cpu/cpu0/cpufreq`
        
    * Maybe it's related to AVX
      
        * Yes, it is! AVX lower CPU frequency
        
            > There are three frequency levels, so-called *licenses*, from fastest to slowest: L0, L1 and L2. L0 is the "nominal" speed you'll see written on the box: when the chip says "3.5 GHz turbo", they are referring to the single-core L0 turbo. L1 is a lower speed sometimes called *AVX turbo* or *AVX2 turbo*5, originally associated with AVX and AVX2 instructions1. L2 is a lower speed than L1, sometimes called "AVX-512 turbo".
        
        * [SIMD instructions lowering CPU frequency](https://stackoverflow.com/questions/56852812/simd-instructions-lowering-cpu-frequency)
        
        * [wikichip: xeon silver 4210](https://en.wikichip.org/wiki/intel/xeon_silver/4210)
        
            ![1578678688267](typora-images-assets/1578678688267.png)

* Will `AVX2` have better performance?

    * > Workloads that execute Intel AVX-512 instructions as a large proportion of their whole instruction count can gain performance compared to Intel AVX2 instructions, even though they may operate at a lower frequency. It is not always easy to predict whether a program’s performance will improve from building it to target Intel AVX-512 instructions. [blog](https://lemire.me/blog/2018/04/19/by-how-much-does-avx-512-slow-down-your-cpu-a-first-experiment/)

* Is `AVX-512` used or not?

    * Yes, it is! See last question.

* Why `flops_PC = 16` ?

* How to futher optimize it by `CCFLAGS` in `Make.linux`?

    [ICC 19.1: Alphabetical List of Compiler Options](https://software.intel.com/en-us/cpp-compiler-developer-guide-and-reference-alphabetical-list-of-compiler-options)

    * `-march=cascadelake -mtune=cascadelake -mavx -mavx512f -mavx512dq -mavx512cd -mavx512bw -mavx512vl -msse4.2 -msse4.1 -msse3`
        * `Not work`
    * `-fno-omit-frame-pointer -foptimize-sibling-calls -fno-protect-parens -global-hoist -ip -ipo0 -mtune=core-avx2 `
        * `Not work`
    * `-march=skylake-avx512`  `-mtune=skylake-avx512`
        * `Not test`
    * My first conclusion is comptimizing the HPL by `CCFLAGS` do little works. Since the main performance determinded parts are `blas` and `HPL.dat` .