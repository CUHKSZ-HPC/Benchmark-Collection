# A simple Installation guidance for Falcon

### Related Website:
* [Falcon Installation Reference](https://github.com/PacificBiosciences/FALCON-integrate/wiki/Installation)
* Memory is a important issue. If you memory is not large enough, you will get [LAmerge: Out of memory](https://github.com/PacificBiosciences/FALCON-integrate/issues/119) error. 

### Installation Environment:
    Ubuntu 18.04.2 LTS, 4.18.0-15-generic, x86_64, VirtualBox, 6GB Memory

### 0. Install dependency
    sudo apt install -y git nim python-dev curl

### 1. Follow the Instruction in [Falcon Installation Reference](https://github.com/PacificBiosciences/FALCON-integrate/wiki/Installation)
    export GIT_SYM_CACHE_DIR=~/.git-sym-cache # to speed things up
    git clone git://github.com/PacificBiosciences/FALCON-integrate.git
    cd FALCON-integrate
    git checkout develop
    git submodule update --init --recursive
    make init
    source env.sh
    make config-edit-user
    make -j all

### 2. Run a simple test
    source env.sh
    make test

##### The output will be something like:
	make -C ./FALCON-make/ test
	make[1]: Entering directory '/home/levi/Desktop/FALCON-integrate/FALCON-make'
	make -C /home/levi/Desktop/FALCON-integrate/FALCON-examples test
	make[2]: Entering directory '/home/levi/Desktop/FALCON-integrate/FALCON-examples'
	python -c 'import pypeflow.pwatcher_workflow; print pypeflow.pwatcher_workflow'
	<module 'pypeflow.pwatcher_workflow' from '/home/levi/Desktop/FALCON-integrate/pypeFLOW/pypeflow/pwatcher_workflow.pyc'>
	python -c 'import falcon_kit; print falcon_kit.falcon'
	<CDLL '/home/levi/Desktop/FALCON-integrate/FALCON/ext_falcon.so', handle 55beeec9fd10 at 7f6b6dfd9550>
	make run-synth0
	make[3]: Entering directory '/home/levi/Desktop/FALCON-integrate/FALCON-examples'
	git-sym update run/synth0
	-> in dir 'run/synth0'
	<- back to dir '/home/levi/Desktop/FALCON-integrate/FALCON-examples'
	symlink: 'run/synth0/data/synth5k'
	-> in dir '/home/levi/Desktop/FALCON-integrate/.git/modules/FALCON-examples/git-sym-local/links'
	<- back to dir '/home/levi/Desktop/FALCON-integrate/FALCON-examples'
	git-sym show run/synth0
	-> in dir 'run/synth0'
	<- back to dir '/home/levi/Desktop/FALCON-integrate/FALCON-examples'
	symlink: 'run/synth0/data/synth5k'
	/ run/synth0/data/synth5k	.git-sym/synth5k.2016-11-02
	git-sym check run/synth0
	-> in dir 'run/synth0'
	<- back to dir '/home/levi/Desktop/FALCON-integrate/FALCON-examples'
	symlink: 'run/synth0/data/synth5k'
	cd run/synth0; rm -rf 0-rawreads/ 1-preads_ovl/ 2-asm-falcon/; ls -l; fc_run.py fc_run.cfg logging.ini
	total 1636
	-rwxr-xr-x 1 levi levi    1067 10月 16 10:28 check.py
	-rw-r--r-- 1 levi levi    2367 10月 16 12:33 config.json
	drwxr-xr-x 2 levi levi    4096 10月 16 10:28 data
	-rw-r--r-- 1 levi levi 1589744 10月 16 12:33 fc.log
	-rw-r--r-- 1 levi levi     251 10月 16 10:28 fc_many.cfg
	-rw-r--r-- 1 levi levi    1611 10月 16 10:28 fc_preads.cfg
	-rw-r--r-- 1 levi levi    1922 10月 16 11:34 fc_run.cfg
	-rw-r--r-- 1 levi levi    1922 10月 16 11:09 fc_run.cfg_backup
	-rw-r--r-- 1 levi levi   21585 10月 16 12:33 foo.snake
	-rw-r--r-- 1 levi levi      28 10月 16 10:28 input.fofn
	-rw-r--r-- 1 levi levi     461 10月 16 10:28 logging.ini
	-rw-r--r-- 1 levi levi     770 10月 16 10:28 logging.json
	-rw-r--r-- 1 levi levi    1026 10月 16 10:28 logging.many.ini
	-rw-r--r-- 1 levi levi     332 10月 16 10:28 makefile
	-rw-r--r-- 1 levi levi      56 10月 16 10:28 preads.fofn
	[3192]$('lfs setstripe -c 12 /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0')
	sh: 1: lfs: not found
	[3192]WARNING: Call 'lfs setstripe -c 12 /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0' returned 32512.
	2019-10-16 04:33:27,638[INFO] Setup logging from file "logging.ini".
	2019-10-16 04:33:27,638[INFO] fc_run started with configuration fc_run.cfg
	2019-10-16 04:33:27,639[INFO]  No target specified, assuming "assembly" as target 
	2019-10-16 04:33:27,640[INFO] cfg=
	{
	  "General": {
		"LA4Falcon_preload": "true",
		"falcon_sense_option": "--output-multi --min-idt 0.70 --min-cov 1 --max-n-read 20000 --n-core 0",
		"genome_size": "5000",
		"input_fofn": "input.fofn",
		"input_type": "raw",
		"length_cutoff_pr": "1",
		"overlap_filtering_setting": "--max-diff 10000 --max-cov 100000 --min-cov 1 --min-len 1 --bestn 1000 --n-core 0",
		"ovlp_DBsplit_option": "-a -x5 -s50",
		"ovlp_HPCdaligner_option": "-v -B4 -t50 -h1 -e.99 -l1 -s1000",
		"pa_DBsplit_option": "-a -x5 -s.065536",
		"pa_HPCdaligner_option": "-v -B4 -t50 -h1 -e.99 -w1 -l1 -s1000",
		"seed_coverage": "20"
	  },
	  "LA4Falcon_preload": true,
	  "avoid_text_file_busy": true,
	  "dazcon": false,
	  "falcon_sense_greedy": false,
	  "falcon_sense_option": "--output-multi --min-idt 0.70 --min-cov 1 --max-n-read 20000 --n-core 0",
	  "falcon_sense_skip_contained": false,
	  "fc_ovlp_to_graph_option": " --min_len 1",
	  "genome_size": 5000,
	  "input_fofn": "input.fofn",
	  "input_type": "raw",
	  "install_prefix": "/usr",
	  "job.defaults": {
		"JOB_QUEUE": "default7",
		"job_name_style": "1",
		"njobs": "2",
		"pwatcher_type": "blocking",
		"stop_all_jobs_on_failure": "true",
		"submit": "bash -c ${JOB_SCRIPT} >| ${JOB_STDOUT} 2>| ${JOB_STDERR}",
		"use_tmpdir": false
	  },
	  "job.step.asm": {
		"NPROC": "24"
	  },
	  "job.step.cns": {
		"NPROC": "8"
	  },
	  "job.step.da": {
		"NPROC": "8"
	  },
	  "job.step.la": {
		"NPROC": "2"
	  },
	  "job.step.pda": {
		"NPROC": "8"
	  },
	  "job.step.pla": {
		"NPROC": "2"
	  },
	  "length_cutoff": -1,
	  "length_cutoff_pr": 1,
	  "overlap_filtering_setting": "--max-diff 10000 --max-cov 100000 --min-cov 1 --min-len 1 --bestn 1000 --n-core 0",
	  "ovlp_DBsplit_option": "-a -x5 -s50",
	  "ovlp_HPCdaligner_option": "-v -B4 -t50 -h1 -e.99 -l1 -s1000",
	  "pa_DBdust_option": "",
	  "pa_DBsplit_option": "-a -x5 -s.065536",
	  "pa_HPCdaligner_option": "-v -B4 -t50 -h1 -e.99 -w1 -l1 -s1000",
	  "pa_HPCrepmask_1_option": " -g1 -c20 -mtan",
	  "pa_HPCrepmask_2_option": " -g10 -c15 -mtan -mrep1",
	  "pa_HPCtanmask_option": "",
	  "pa_damasker_HPCdaligner_option": " -mtan -mrep1 -mrep10",
	  "pa_dazcon_option": "-j 4 -x -l 500",
	  "pa_repmask_levels": 0,
	  "pa_use_repmask": false,
	  "pa_use_tanmask": false,
	  "seed_coverage": 20.0,
	  "skip_checks": false,
	  "target": "assembly"
	}
	2019-10-16 04:33:27,640[INFO] In simple_pwatcher_bridge, pwatcher_impl=<module 'pwatcher.blocking' from '/home/levi/Desktop/FALCON-integrate/pypeFLOW/pwatcher/blocking.pyc'>
	2019-10-16 04:33:27,640[INFO] job_type='local', (default)job_defaults={'JOB_QUEUE': 'default7', 'pwatcher_type': 'blocking', 'use_tmpdir': False, 'job_name_style': '1', 'submit': 'bash -c ${JOB_SCRIPT} >| ${JOB_STDOUT} 2>| ${JOB_STDERR}', 'stop_all_jobs_on_failure': 'true', 'njobs': '2'}, use_tmpdir=False, squash=True, job_name_style='1'
	2019-10-16 04:33:27,641[INFO] Setting max_jobs to 2; was None
	2019-10-16 04:33:27,642[INFO] Setting max_jobs to 2; was 2
	2019-10-16 04:33:27,646[INFO] Setting max_jobs to 2; was 2
	2019-10-16 04:33:27,650[INFO] Num unsatisfied: 2, graph: 2
	2019-10-16 04:33:27,650[INFO] About to submit: Node(0-rawreads)
	2019-10-16 04:33:27,652[INFO] Popen: 'bash -c /home/levi/Desktop/FALCON-integrate/pypeFLOW/pwatcher/mains/job_start.sh >| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/0-rawreads/run-P0_0_rawreads_5372d62ecbf4ce03c39ed9ec1761b97d.bash.stdout 2>| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/0-rawreads/run-P0_0_rawreads_5372d62ecbf4ce03c39ed9ec1761b97d.bash.stderr'
	2019-10-16 04:33:27,654[INFO] (slept for another 0.0s -- another 1 loop iterations)
	2019-10-16 04:33:27,975[INFO] recently_satisfied:
	set([Node(0-rawreads)])
	2019-10-16 04:33:27,976[INFO] Num satisfied in this iteration: 1
	2019-10-16 04:33:27,976[INFO] Num still unsatisfied: 1
	2019-10-16 04:33:27,976[INFO] About to submit: Node(0-rawreads/daligner-split)
	2019-10-16 04:33:27,981[INFO] Popen: '/bin/bash -C /home/levi/Desktop/FALCON-integrate/pypeFLOW/pwatcher/mains/job_start.sh >| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/0-rawreads/daligner-split/run-P0_daligner_split_e2a741f5ea4bd17b169a066490dd415b.bash.stdout 2>| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/0-rawreads/daligner-split/run-P0_daligner_split_e2a741f5ea4bd17b169a066490dd415b.bash.stderr'
	2019-10-16 04:33:27,984[INFO] (slept for another 0.3s -- another 3 loop iterations)
	2019-10-16 04:33:28,287[INFO] recently_satisfied:
	set([Node(0-rawreads/daligner-split)])
	2019-10-16 04:33:28,287[INFO] Num satisfied in this iteration: 1
	2019-10-16 04:33:28,287[INFO] Num still unsatisfied: 0
	2019-10-16 04:33:28,321[INFO] Setting max_jobs to 2; was 2
	2019-10-16 04:33:28,327[INFO] Num unsatisfied: 7, graph: 9
	2019-10-16 04:33:28,327[INFO] About to submit: Node(0-rawreads/daligner-chunks/j_0001)
	2019-10-16 04:33:28,327[INFO] About to submit: Node(0-rawreads/daligner-chunks/j_0000)
	2019-10-16 04:33:28,330[INFO] Popen: '/bin/bash -C /home/levi/Desktop/FALCON-integrate/pypeFLOW/pwatcher/mains/job_start.sh >| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/0-rawreads/daligner-chunks/j_0000/run-P0_j_0000_77085d106ac4af6b5a41e8db1fa505d6.bash.stdout 2>| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/0-rawreads/daligner-chunks/j_0000/run-P0_j_0000_77085d106ac4af6b5a41e8db1fa505d6.bash.stderr'
	2019-10-16 04:33:28,333[INFO] Popen: '/bin/bash -C /home/levi/Desktop/FALCON-integrate/pypeFLOW/pwatcher/mains/job_start.sh >| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/0-rawreads/daligner-chunks/j_0001/run-P0_j_0001_865bcc16552a80d90ac966151399825c.bash.stdout 2>| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/0-rawreads/daligner-chunks/j_0001/run-P0_j_0001_865bcc16552a80d90ac966151399825c.bash.stderr'
	2019-10-16 04:33:28,335[INFO] (slept for another 0.0s -- another 1 loop iterations)
	2019-10-16 04:33:28,638[INFO] recently_satisfied:
	set([Node(0-rawreads/daligner-chunks/j_0001),
		 Node(0-rawreads/daligner-chunks/j_0000)])
	2019-10-16 04:33:28,638[INFO] Num satisfied in this iteration: 2
	2019-10-16 04:33:28,639[INFO] Num still unsatisfied: 5
	2019-10-16 04:33:28,639[INFO] About to submit: Node(0-rawreads/daligner-runs/j_0000)
	2019-10-16 04:33:28,639[INFO] About to submit: Node(0-rawreads/daligner-runs/j_0001)
	2019-10-16 04:33:28,644[INFO] Popen: 'bash -c /home/levi/Desktop/FALCON-integrate/pypeFLOW/pwatcher/mains/job_start.sh >| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/0-rawreads/daligner-runs/j_0000/run-P0_j_0000_1ef1d9652ffc4ad338e3ee5db7d21dbb.bash.stdout 2>| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/0-rawreads/daligner-runs/j_0000/run-P0_j_0000_1ef1d9652ffc4ad338e3ee5db7d21dbb.bash.stderr'
	2019-10-16 04:33:28,650[INFO] (slept for another 0.3s -- another 3 loop iterations)
	2019-10-16 04:33:28,650[INFO] Popen: 'bash -c /home/levi/Desktop/FALCON-integrate/pypeFLOW/pwatcher/mains/job_start.sh >| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/0-rawreads/daligner-runs/j_0001/run-P0_j_0001_e35c48d1d5857feed05bf0f113a90712.bash.stdout 2>| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/0-rawreads/daligner-runs/j_0001/run-P0_j_0001_e35c48d1d5857feed05bf0f113a90712.bash.stderr'
	2019-10-16 04:33:28,957[INFO] recently_satisfied:
	set([Node(0-rawreads/daligner-runs/j_0001),
		 Node(0-rawreads/daligner-runs/j_0000)])
	2019-10-16 04:33:28,957[INFO] Num satisfied in this iteration: 2
	2019-10-16 04:33:28,957[INFO] Num still unsatisfied: 3
	2019-10-16 04:33:28,957[INFO] About to submit: Node(0-rawreads/daligner-gathered)
	2019-10-16 04:33:28,961[INFO] Popen: '/bin/bash -C /home/levi/Desktop/FALCON-integrate/pypeFLOW/pwatcher/mains/job_start.sh >| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/0-rawreads/daligner-gathered/run-P0_daligner_gathered_aadba9556bfc3a63c152f711974726ea.bash.stdout 2>| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/0-rawreads/daligner-gathered/run-P0_daligner_gathered_aadba9556bfc3a63c152f711974726ea.bash.stderr'
	2019-10-16 04:33:28,962[INFO] (slept for another 0.3s -- another 3 loop iterations)
	2019-10-16 04:33:29,267[INFO] recently_satisfied:
	set([Node(0-rawreads/daligner-gathered)])
	2019-10-16 04:33:29,267[INFO] Num satisfied in this iteration: 1
	2019-10-16 04:33:29,267[INFO] Num still unsatisfied: 2
	2019-10-16 04:33:29,268[INFO] About to submit: Node(0-rawreads/daligner-intermediate-gathered-las)
	2019-10-16 04:33:29,272[INFO] Popen: '/bin/bash -C /home/levi/Desktop/FALCON-integrate/pypeFLOW/pwatcher/mains/job_start.sh >| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/0-rawreads/daligner-intermediate-gathered-las/run-P0_daligner_intermediate_gathered_las_a928097170e86cee009ec84f34adf2b1.bash.stdout 2>| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/0-rawreads/daligner-intermediate-gathered-las/run-P0_daligner_intermediate_gathered_las_a928097170e86cee009ec84f34adf2b1.bash.stderr'
	2019-10-16 04:33:29,374[INFO] (slept for another 0.4s -- another 4 loop iterations)
	2019-10-16 04:33:29,576[INFO] recently_satisfied:
	set([Node(0-rawreads/daligner-intermediate-gathered-las)])
	2019-10-16 04:33:29,576[INFO] Num satisfied in this iteration: 1
	2019-10-16 04:33:29,577[INFO] Num still unsatisfied: 1
	2019-10-16 04:33:29,577[INFO] About to submit: Node(0-rawreads/las-merge-split)
	2019-10-16 04:33:29,582[INFO] Popen: '/bin/bash -C /home/levi/Desktop/FALCON-integrate/pypeFLOW/pwatcher/mains/job_start.sh >| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/0-rawreads/las-merge-split/run-P0_las_merge_split_e6ed84f1c018fb2a3a2e3a727cf95d9b.bash.stdout 2>| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/0-rawreads/las-merge-split/run-P0_las_merge_split_e6ed84f1c018fb2a3a2e3a727cf95d9b.bash.stderr'


	...
	...
	...


	2019-10-16 04:33:39,394[INFO] Popen: 'bash -c /home/levi/Desktop/FALCON-integrate/pypeFLOW/pwatcher/mains/job_start.sh >| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/2-asm-falcon/run-PP_2_asm_falcon_6cfb90b0b558b8930c2aab50b65aa0be.bash.stdout 2>| /home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0/2-asm-falcon/run-PP_2_asm_falcon_6cfb90b0b558b8930c2aab50b65aa0be.bash.stderr'
	2019-10-16 04:33:39,497[INFO] (slept for another 0.4s -- another 5 loop iterations)
	2019-10-16 04:33:40,901[INFO] recently_satisfied:
	set([Node(2-asm-falcon)])
	2019-10-16 04:33:40,901[INFO] Num satisfied in this iteration: 1
	2019-10-16 04:33:40,901[INFO] Num still unsatisfied: 0
	make[3]: Leaving directory '/home/levi/Desktop/FALCON-integrate/FALCON-examples'
	make -C run/synth0 test
	make[3]: Entering directory '/home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0'
	./check.py
	shifted by 3269 (rc)
	make[3]: Leaving directory '/home/levi/Desktop/FALCON-integrate/FALCON-examples/run/synth0'
	make[2]: Leaving directory '/home/levi/Desktop/FALCON-integrate/FALCON-examples'
	make[1]: Leaving directory '/home/levi/Desktop/FALCON-integrate/FALCON-make
