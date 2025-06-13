Mapping of Illumina reads to assembly 
-------------------------------------

We are mapping the Illumina reads to the largest contig of our assembly with BWA. BWA is a software package for mapping low-divergent sequences against a large reference genome. It consists of three algorithms: BWA-backtrack, BWA-SW and BWA-MEM. The first algorithm is designed for Illumina sequence reads up to 100bp, while the rest two for longer sequences ranged from 70bp to 1Mbp. BWA-MEM and BWA-SW share similar features such as long-read support and split alignment, but BWA-MEM, which is the latest, is generally recommended for high-quality queries as it is faster and more accurate.

The first step is to create an index on the assembly, to allow mapping::
  
  bwa index ~/workdir/assembly/assembly.contigs.fasta
  
Then we are mapping all reads to the contig. Note that we are shortening the process of creating a sorted and indexed bam file by piping the output of bwa directly to samtools, thereby avoiding temporary files::

  cd ~/workdir/
  mkdir illumina_mapping

  bwa mem -t 14 ~/workdir/assembly/assembly.contigs.fasta ~/workdir/data/illumina/Illumina_R1.fastq.gz ~/workdir/data/illumina/Illumina_R2.fastq.gz | samtools view - -Sb | samtools sort - -@14 -o ~/workdir/illumina_mapping/mapping.sorted.bam
  
  samtools index ~/workdir/illumina_mapping/mapping.sorted.bam

References
^^^^^^^^^^

**BWA** http://bio-bwa.sourceforge.net/
