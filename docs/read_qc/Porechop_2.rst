Porechop(2)
------


  cd ~/workdir
  porechop -i ~/workdir/basecall/basecall.fastq.gz -t 14 -v 2 -o ~/workdir/basecall/basecall_trimmed.fastq.gz > porechop.log

Let's inspect the log file::

  more porechop.log 
  
So here, the following adapters were found and trimmed::

  Trimming adapters from read ends
    Rapid_adapter: GTTTTCGCATTTATCGTGAAACGCTTTCGCGTTTTTCGTGCGCCGCTTCA
         BC04_rev: TAGGGAAACACGATAGAATCCGAA
             BC04: TTCGGATTCTATCGTGTTTCCCTA
         BC11_rev: TCCATTCCCTCCGATAGATGAAAC
             BC11: GTTTCATCTATCGGAGGGAATGGA
             BC06: TTCTCGCAAAGGCAGAAAGTAGTC
         BC06_rev: GACTACTTTCTGCCTTTGCGAGAA

To see how many reads were trimmed, grep for reads::

  grep reads porechop.log
  
  52,536 reads loaded
  51,299 / 52,536 reads had adapters trimmed from their start (5,257,865 bp removed)
  4,890 / 52,536 reads had adapters trimmed from their end (47,632 bp removed)
  794 / 52,536 reads were split based on middle adapters


We will again look into the results of FastQC::

  mkdir -p ~/workdir/fastqc/nanopore_fastqc_trimmed/
  fastqc -t 14 -o  ~/workdir/fastqc/nanopore_fastqc_trimmed/  ~/workdir/basecall/basecall_trimmed.fastq.gz
  
References
^^^^^^^^^^

**FastQC** https://www.bioinformatics.babraham.ac.uk/projects/fastqc/

**Porechop** https://github.com/rrwick/Porechop
