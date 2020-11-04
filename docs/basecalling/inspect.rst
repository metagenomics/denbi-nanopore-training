Inspect the output
------------------

The directory contains the following output::

  ls -l ~/workdir/data_artic/basecall_tiny_<number>
  
  total 4544
  -rw-rw-r-- 1 ubuntu ubuntu 2356539 Nov  4 10:03 fastq_runid_ca07d29b288c9f8881387334d8fc7d195fb11339_0_0.fastq.gz
  -rw-rw-r-- 1 ubuntu ubuntu 1209378 Nov  4 10:03 guppy_basecaller_log-2020-11-04_09-54-21.log
  -rw-rw-r-- 1 ubuntu ubuntu  969796 Nov  4 10:03 sequencing_summary.txt
  -rw-rw-r-- 1 ubuntu ubuntu  108243 Nov  4 10:03 sequencing_telemetry.js

So we have one fastq file in our directory - since we started with one fast5 file. Ususally, we should merge all resulting fastq files into a single file::

  cat ~/workdir/data_artic/basecall_tiny_<number>/*.fastq.gz > ~/workdir/data_artic/basecall_tiny_<number>.fastq.gz

In order to get the number of reads in our fastq file, we can count the number of lines and divide by 4::

  zcat ~/workdir/data_artic/basecall_tiny_<number>.fastq.gz | wc -l | awk '{print $1/4}'
  
And to get the number of bases::

   zcat ~/workdir/data_artic/basecall_tiny_<number>.fastq.gz | paste - - - - | cut -f 2 | tr -d '\n' | wc -c

With a genome size of ~30k we get a coverage of::

   zcat ~/workdir/data_artic/basecall_tiny_<number>.fastq.gz | paste - - - - | cut -f 2 | tr -d '\n' | wc -c | awk '{print $1/30000}'
