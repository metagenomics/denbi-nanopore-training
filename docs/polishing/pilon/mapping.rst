Mapping of Illumina reads to assembly 
-------------------------------------

We are mapping the Illumina reads to the largest contig of our assembly. The first step is to create an index on the largestContig, to allow mapping::
  
  bwa index ~/workdir/canu_assembly/largestContig.fasta
  
Then we are mapping all reads to the contig. Note that we are shortening the process of creating a sorted and indexed bam file by piping the output of bwa directly to samtools, thereby avoiding temporary files::

  cd
  mkdir Illumina_mappings

  bwa mem -t 16 ~/workdir/canu_assembly/largestContig.fasta ~/Data/Illumina/TSPf_R1.fastq.gz ~/Data/Illumina/TSPf_R2.fastq.gz | samtools view - -Sb | samtools sort - -@16 -o sorted > ~/workdir/Illumina_mappings/WGS.sorted.bam
  samtools index ~/workdir/Illumina_mappings/WGS.sorted.bam
  
  bwa mem -t 16 ~/workdir/canu_assembly/largestContig.fasta ~/Data/Illumina/MP2.fastq.fwd ~/Data/Illumina/MP2.fastq.rev | samtools view - -Sb | samtools sort - -@16 -o sorted > ~/workdir/Illumina_mappings/MP.sorted.bam
  samtools index ~/workdir/Illumina_mappings/MP.sorted.bam
