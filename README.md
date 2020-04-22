# AP-Collection
In a formal supercomputer challenge competition, each participating team should submit a design of their clusters, and also run some benchmark and applications to compete on the performance of their clusters. Every time we finish a competition, we should store the distributed applications, and also record methods those we have tried to get our submitted result. It's a good way to learn, and also a good way to pass our knowledge to our fellows. So, in this repo we store some common benchmarks and applications in previous competitions. Through inspecting these applications, we hope our current and future team members can study and get familiar with the competition. 

# Table of Contents

- [/falcon](https://github.com/CUHK-SZ-SC/AP-Collection/tree/master/falcon): Falcon Genome Assembly Benchmark
- [/hpcc](https://github.com/CUHK-SZ-SC/AP-Collection/tree/master/hpcc): HPC Challenge Benchmark
- [/hpcg](https://github.com/CUHK-SZ-SC/AP-Collection/tree/master/hpcg): High Performance Conjugate Gradients Benchmark
- [/hpl](https://github.com/CUHK-SZ-SC/AP-Collection/tree/master/hpl): High Performance Linpack Benchmark 

# How to Submit New Application or Benchmark

Following is a sample structure to store an application or benchmark

- **/name**: formal name of the application or benchmark (sub-directory is allowed to make differences among competitions)
  - **/pkg/**: whole package of the original application
  - **/opt/** (optional): optimized package of application 
  - **/data/** (optional): possible dataset or result set
  - **/man/**: how to run the application, how to obtain the benchmark result
  - **/doc/**: collective paperwork in group seminars (or proposal)
  - **/ref/**: useful papers and documents
  - **/readme**: write some informative introduction to the application
  - **/contrib**: author/director for collecting this application
