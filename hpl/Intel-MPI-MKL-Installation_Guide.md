## Install `Intel MPI` and `Intel MKL (blas)`

**Environment: `Ubuntu 18.04 LTS`**

You should Install [Intel® Parallel Studio XE for Linux*](https://software.intel.com/en-us/parallel-studio-xe/choose-download/student-linux-fortran), which Included `Intel® C++ Compiler`  `Intel® Math Kernel Library`  `Intel® MPI Library`.

The whole package occupies 17 GB, you can custom to only install the needed component.

You can install and use `Intel MKL` separately, but somehow I found it behave strange when not compiled by `icc`.

* ```bash
    # 1. Register and download Intel® Parallel Studio XE for Linux*
    # 2. Get the serious number from Email ( levi: S477-WWH887FH, you may want to ask him first )
    # 3. Install
    ./install.sh
    ```

* ```bash
    # Test
    source /opt/intel/compilers_and_libraries_2020.0.166/linux/bin/compilervars.sh intel64
    mpiicc --version
    icc --version
    ```

**Remember to `source` every time you want to use MPI/MKL, or put it in like `.bashrc`**


Also, based on [packages.ubuntu: intel-mkl](https://packages.ubuntu.com/search?keywords=intel-mkl), starting from `Ubuntu 19.04`, you can `apt install intel-mkl`.

