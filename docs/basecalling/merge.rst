Merge fastqs
------------

To make life easier for future computations, we will merge the fastq files into one::

  cat ~/workdir/1D_basecall_small/workspace/pass/*.fastq > ~/workdir/1D_basecall_small.fastq
  cat ~/workdir/1D2_basecall_small/1dsq_analysis/workspace/pass/*.fastq > ~/workdir/1D2_basecall_small.fastq
  cat ~/workdir/1D_basecall/workspace/pass/*.fastq > ~/workdir/1D_basecall.fastq
  cat ~/workdir/1D2_basecall/1dsq_analysis/workspace/pass/*.fastq > ~/workdir/1D2_basecall.fastq



