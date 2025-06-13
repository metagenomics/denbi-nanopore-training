Inspect the output
------------------

The directory contains the following output::

  ls -l ~/workdir/basecall_tiny/
  
  total 4456
  -rw-rw-r-- 1 ubuntu ubuntu 3995875 Sep 18 09:56 fastq_runid_b110eefd3ba5e91817c69585fcd2257218eeb796_0.fastq.gz
  -rw-rw-r-- 1 ubuntu ubuntu  261383 Sep 18 09:56 guppy_basecaller_log-2019-09-18_09-49-20.log
  -rw-rw-r-- 1 ubuntu ubuntu  179651 Sep 18 09:56 sequencing_summary.txt
  -rw-rw-r-- 1 ubuntu ubuntu  121167 Sep 18 09:56 sequencing_telemetry.js

So we have one fastq file in our directory - since we started with one fast5 file. Ususally, we should merge all resulting fastq files into a single file::

  cat ~/workdir/basecall_tiny/*.fastq.gz > ~/workdir/basecall_tiny/basecall.fastq.gz

In order to get the number of reads in our fastq file, we can count the number of lines and divide by 4::

  zcat ~/workdir/basecall_tiny/basecall.fastq.gz | wc -l | awk '{print $1/4}'
