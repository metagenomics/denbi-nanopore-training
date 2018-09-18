Mapping of Illumina reads to assembly 
-------------------------------------

We are mapping the Illumina reads to the largest contig of our assembly with BWA. BWA is a software package for mapping low-divergent sequences against a large reference genome. It consists of three algorithms: BWA-backtrack, BWA-SW and BWA-MEM. The first algorithm is designed for Illumina sequence reads up to 100bp, while the rest two for longer sequences ranged from 70bp to 1Mbp. BWA-MEM and BWA-SW share similar features such as long-read support and split alignment, but BWA-MEM, which is the latest, is generally recommended for high-quality queries as it is faster and more accurate.

The first step is to create an index on the largestContig, to allow mapping::
  
  bwa index ~/workdir/canu_assembly/largestContig.fasta
  
Then we are mapping all reads to the contig. Note that we are shortening the process of creating a sorted and indexed bam file by piping the output of bwa directly to samtools, thereby avoiding temporary files::

  cd ~/workdir/
  mkdir Illumina_mappings

  bwa mem -t 14 ~/workdir/canu_assembly/largestContig.fasta ~/workdir/Data/Illumina/TSPf_R1.fastq.gz ~/workdir/Data/Illumina/TSPf_R2.fastq.gz | samtools view - -Sb | samtools sort - -@16 -o ~/workdir/Illumina_mappings/WGS.sorted.bam
  samtools index ~/workdir/Illumina_mappings/WGS.sorted.bam
  
  bwa mem -t 14 ~/workdir/canu_assembly/largestContig.fasta ~/workdir/Data/Illumina/MP2.fastq.fwd ~/workdir/Data/Illumina/MP2.fastq.rev | samtools view - -Sb | samtools sort - -@16 -o ~/workdir/Illumina_mappings/MP.sorted.bam
  samtools index ~/workdir/Illumina_mappings/MP.sorted.bam

References
^^^^^^^^^^

**BWA** http://bio-bwa.sourceforge.net/
