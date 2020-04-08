## Install `Intel MPI` and `Intel MKL (blas)`

**Environment: `Ubuntu 18.04 LTS`**

You should Install [Intel® Parallel Studio XE for Linux*](https://software.intel.com/en-us/parallel-studio-xe/choose-download/student-linux-fortran), which Included `Intel® C++ Compiler`  `Intel® Math Kernel Library`  `Intel® MPI Library`.

The whole package occupies 17 GB, you can custom to only install the needed component.

You can install and use `Intel MKL` separately, but somehow I found it behave strange when not compiled by `icc`.

* ```bash
    # 1. Register and download Intel® Parallel Studio XE for Linux*
    # 2. Get the serious number from Email ( levi: S477-WWH887FH, ask him first if you want )
    # 3.0 apt install cpio
    # 3. Install
    ./install.sh
	# If you don't need vtune profiler, you can skip the prerequirest of GUI
    	# Costum and install `Intel C++ Compiler 19.1` `Intel Math Kernel Library 2020 for C/C++` `Intel MPI Library 2019 Update 6`
    ```
    
* ```bash
    # Test
    source /opt/intel/compilers_and_libraries_2020.0.166/linux/bin/compilervars.sh intel64
    mpiicc --version
    icc --version
    ```

**Remember to `source` every time you want to use MPI/MKL, or put it in like `.bashrc`**


Also, based on [packages.ubuntu: intel-mkl](https://packages.ubuntu.com/search?keywords=intel-mkl), starting from `Ubuntu 19.04`, you can `apt install intel-mkl`.



## Update: 2020-2-11

```bash
	# 3 Install parallel_studio (Install Manually!!!)
	echo "Install Intel® Parallel Studio Manually"
	exit 0
	cd /root/origin/parallel_studio_xe_2020_cluster_edition_online/
	apt install cpio 
			
	# To see download speed: apt install nethogs
	# To see download process (total 711M): watch -d -n1 "ls /tmp/root/parallel_studio_xe_2020_cluster_edition/parallel_studio_xe_2020_cluster_edition/rpm -lah | grep tota"
	# If download speed is unpleasant: srync -av rpm_from_server_already_finished_Downlaod rpm_server_who_need_to_Download 
		# This will work, install.sh is smart. Of cause a better way is tar -> scp -> untar
			# rpm dir: /tmp/root/parallel_studio_xe_2020_cluster_edition/parallel_studio_xe_2020_cluster_edition/rpm

	mkdir -p /tmp/root/parallel_studio_xe_2020_cluster_edition/parallel_studio_xe_2020_cluster_edition/
	tar zxvf /root/origin/parallel_studio_xe_2020_cluster_edition_rpm.tar.gz -C /tmp/root/parallel_studio_xe_2020_cluster_edition/parallel_studio_xe_2020_cluster_edition/

	./install.sh
		# 1 > q > accept > 2 > 1 > 1 > serial number > 1 > q > 
		#  2 > 1 > 3 > 3 > a > a > 7 2 1 > 9 2 1 > 14 2 1> 1 > y > 1

	# To skip interactive mode (by loading existing cfg file):
		# cp /root/origin/intelCpp_intelMKL_intelMPI.cfg ./
		# ./install.sh --silent=intelCpp_intelMKL_intelMPI.cfg
			# To generate cfg file: ./install.sh --duplicate=test.cfg
			# But it is not smart, it won't use the already downloaded rpm, wonders.

	# The log is under /tmp/

	source /opt/intel/compilers_and_libraries_2020.0.166/linux/bin/compilervars.sh intel64
	echo "source /opt/intel/compilers_and_libraries_2020.0.166/linux/bin/compilervars.sh intel64" >> /root/.bashrc

	# Some other ways to install Parallel Studio:
		# https://cmg.cds.iisc.ac.in/parmoon/wordpress/wp-content/uploads/2019/10/parallel-studio-xe-2019u4-install-guide-lin.pdf
		# https://software.intel.com/en-us/articles/installing-intel-parallel-studio-xe-runtime-2019-using-apt-repository
		# https://software.intel.com/en-us/articles/installing-intel-parallel-studio-xe-runtime-2019-using-apt-repository
	exit 0
```