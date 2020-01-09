#### Install `Intel MPI` and `Intel MKL (blas)`

You should Install [Intel® Parallel Studio XE for Linux*](https://software.intel.com/en-us/parallel-studio-xe/choose-download/student-linux-fortran), which Included `Intel® C++ Compiler`  `Intel® Math Kernel Library`  `Intel® MPI Library`.

It is sad that it also include lots of unrelevent things, which occupys totally 17 GB. But this is the only way to install `Intel® C++ Compiler (icc)` , which is needed by `Intel MPI`.

You can install and use `Intel MKL` seperately, but it's not gona give you high score without `Intel MPI`.

* ```bash
    # 1. Register and download Intel® Parallel Studio XE for Linux*
    # 2. Get the serious number from Email ( my is S477-WWH887FH )
    # 3. Install
    ./install.sh
    ```

* ```bash
    # Test
    source /opt/intel/compilers_and_libraries_2020.0.166/linux/bin/compilervars.sh intel64
    mpiicc --version
    icc --version
    ```

    

