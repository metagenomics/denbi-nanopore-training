Inspect the output
------------------

Both directories contain a number of fastq files::

  ls -lh ~/workdir/1D_basecall_small/workspace/pass/
  
  total 12M
  -rw-rw-r-- 1 ubuntu ubuntu 4.6M Nov 13 10:17 fastq_runid_04d71dafbed4e1a2c29d48873533c94070985063_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu  98K Nov 13 10:17 fastq_runid_307482bb8322e11a4f92efefd01364754f9c271f_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 664K Nov 13 10:17 fastq_runid_492de34daf0e1e3648eed3c976ecf01b9ae1a60f_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 3.6M Nov 13 10:17 fastq_runid_940cafc8dbea461f32589d22e3095264700230fb_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 1.4M Nov 13 10:17 fastq_runid_cdd5fefcf4478e23e0628e437f145a503cffa888_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 865K Nov 13 10:17 fastq_runid_fa18a6a6c046ba9c4e91a6381be34a7eb06afbff_0.fastq

The D1^2 basecalling also creates additional fast5 data in the workspace. Keep that in mind, when disk space is limited. ::

  ls -lh ~/workdir/1D2_basecall_small/workspace/
  
  total 13M
  drwxrwxr-x 2 ubuntu ubuntu 144K Nov 13 10:19 0   <-- additional fast5 data
  -rw-rw-r-- 1 ubuntu ubuntu 4.6M Nov 13 10:19 fastq_runid_04d71dafbed4e1a2c29d48873533c94070985063_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 101K Nov 13 10:19 fastq_runid_307482bb8322e11a4f92efefd01364754f9c271f_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 778K Nov 13 10:19 fastq_runid_492de34daf0e1e3648eed3c976ecf01b9ae1a60f_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 4.2M Nov 13 10:19 fastq_runid_940cafc8dbea461f32589d22e3095264700230fb_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 1.6M Nov 13 10:19 fastq_runid_cdd5fefcf4478e23e0628e437f145a503cffa888_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 961K Nov 13 10:19 fastq_runid_fa18a6a6c046ba9c4e91a6381be34a7eb06afbff_0.fastq

The workspace directory above contains the 1D basecalling, whereas the 1D² basecalling is located in::

  ls -lh ~/workdir/1D2_basecall_small/1dsq_analysis/workspace/pass/

  total 1180
  -rw-rw-r-- 1 ubuntu ubuntu 559842 Nov 13 10:21 fastq_runid_04d71dafbed4e1a2c29d48873533c94070985063_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu  61776 Nov 13 10:21 fastq_runid_492de34daf0e1e3648eed3c976ecf01b9ae1a60f_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 447404 Nov 13 10:22 fastq_runid_940cafc8dbea461f32589d22e3095264700230fb_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu  98055 Nov 13 10:22 fastq_runid_cdd5fefcf4478e23e0628e437f145a503cffa888_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu  31740 Nov 13 10:22 fastq_runid_fa18a6a6c046ba9c4e91a6381be34a7eb06afbff_0.fastq



