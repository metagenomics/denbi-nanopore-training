Further polishing with pilon
----------------------------

Before we have a look on the results, we will further polish with pilon.


As usual, we need to map the data to the assembly and run several pilon rounds::

  bwa index ~/racon_medaka/consensus.fasta
  cd ~/workdir
  mkdir illumina_mapping_consensus
  bwa mem -t 14 ~/workdir/racon_medaka/consensus.fasta ~/workdir/data/illumina/Illumina_R1.fastq.gz ~/workdir/data/illumina/Illumina_R2.fastq.gz | samtools view - -Sb | samtools sort - -@14 -o ~/workdir/illumina_mapping_consensus/mapping.sorted.bam
  samtools index ~/workdir/illumina_mapping_consensus/mapping.sorted.bam
  cd ~/workdir
  mkdir racon_medaka_pilon
  java -Xmx32G -jar ~/pilon-1.22.jar --genome racon_medaka/consensus.fasta --fix all --changes --frags illumina_mapping_consensus/mapping.sorted.bam --threads 14 --output racon_medaka_pilon/pilon_round1 | tee racon_medaka_pilon/round1.pilon
  
Repeat several rounds: 
  bwa mem -t 14 ~/workdir/racon_medaka_pilon/pilon_round1.fasta ~/workdir/data/illumina/Illumina_R1.fastq.gz ~/workdir/data/illumina/Illumina_R2.fastq.gz | samtools view - -Sb | samtools sort - -@14 -o ~/workdir/illumina_mapping_consensus/mapping_pilon1.sorted.bam
  samtools index ~/workdir/illumina_mapping_consensus/mapping_pilon1.sorted.bam
  java -Xmx32G -jar ~/pilon-1.22.jar --genome ~/workdir/racon_medaka_pilon/pilon_round1.fasta --fix all --changes --frags illumina_mapping_consensus/mapping_pilon1.sorted.bam --threads 14 --output racon_medaka_pilon/pilon_round2 | tee racon_medaka_pilon/round2.pilon
  

We have also precomputed the polishing for you, if we are short on time::

  cp ...

References
^^^^^^^^^^

**pilon** https://github.com/broadinstitute/pilon/wiki
