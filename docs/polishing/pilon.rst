Assembly polishing with pilon
=============================

Use Illumina data to polish Nanopore assembly (not tested, adapt to cloud)::

  cd
  Mapping mit bwa, sortieren und indexieren
  ~> bwa index ID.contigs.fasta ID.contigs.fasta
  ~> bwa mem -t 16 ID.contigs.fasta /vol/porecourse/DATA/Illumina/TSPf_R1.fastq.gz /vol/porecourse/DATA/Illumina/TSPf_R2.fastq.gz | samtools sort --threads 16 -o WGS.bam
  ~> samtools index WGS.bam
  ~> bwa mem -t 16 ID.contigs.fasta /vol/porecourse/DATA/Illumina/MP2.fastq.fwd /vol/porecourse/DATA/Illumina/MP2.fastq.rev | samtools sort --threads 16 -o MP.bam
  ~> samtools index MP.bam
  ~> java -Xmx80G -jar /vol/porecourse/bin/pilon-1.22.jar --genome ID.contigs.fasta --fix all --changes --frags WGS.bam --jumps MP.bam --threads 16 --output Round1 | tee Round1.pilon
  ~ 15min auf statler
  3-4 Runden bis keine Aenderungen mehr vorkommen.
