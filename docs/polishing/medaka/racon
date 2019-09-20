

racon first::
  
  cd ~/workdir/nanopore_mapping
  samtools view -S mapping.sorted.bam > mapping.sorted.sam
  cd ~/workdir/
  mkdir racon
  racon -t 14 ~/workdir/basecall/basecall.fastq.gz ~/workdir/nanopore_mapping/mapping.sorted.sam ~/workdir/assembly/assembly.contigs.fasta > racon/racon.fasta
