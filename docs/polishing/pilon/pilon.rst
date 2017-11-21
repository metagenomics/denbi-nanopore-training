Call pilon
----------

In the next step, we call pilon with the mappings to polish our assembly::
  
  cd ~/workdir/
  mkdir Pilon
  java -Xmx32G -jar ~/pilon-1.22.jar --genome ~/workdir/canu_assembly/largestContig.fasta --fix all --changes --frags ~/workdir/Illumina_mappings/WGS.sorted.bam --jumps ~/workdir/Illumina_mappings/MP.sorted.bam --threads 16 --output ~/workdir/Pilon/Pilon_round1 | tee ~/workdir/Pilon/round1.pilon
  
Repeat this for 3-4 rounds like this::

  bwa index ~/workdir/Pilon/Pilon_round1.fasta

  bwa mem -t 16 ~/workdir/Pilon/Pilon_round1.fasta ~/Data/Illumina/TSPf_R1.fastq.gz ~/Data/Illumina/TSPf_R2.fastq.gz | samtools view - -Sb | samtools sort - -@16 -o ~/workdir/Illumina_mappings/WGS.sorted.bam
  
  samtools index ~/workdir/Illumina_mappings/WGS.sorted.bam
  
  bwa mem -t 16 ~/workdir/Pilon/Pilon_round1.fasta ~/Data/Illumina/MP2.fastq.fwd ~/Data/Illumina/MP2.fastq.rev | samtools view - -Sb | samtools sort - -@16 -o ~/workdir/Illumina_mappings/MP.sorted.bam
  
  samtools index ~/workdir/Illumina_mappings/MP.sorted.bam
  
  java -Xmx32G -jar ~/pilon-1.22.jar --genome ~/workdir/Pilon/Pilon_round1.fasta --fix all --changes --frags ~/workdir/Illumina_mappings/WGS.sorted.bam --jumps ~/workdir/Illumina_mappings/MP.sorted.bam --threads 16 --output ~/workdir/Pilon/Pilon_round2 | tee ~/workdir/Pilon/round2.pilon
  ...

You can inspect the ``Pilon_roundX.changes`` file to see if there are changes to the previous round.

You can copy further rounds from the precomputed Result directory to your workdir::

  cp -r ~/Results/Pilon/Pilon_round{3,4}* ~/workdir/Pilon/.


References
^^^^^^^^^^

**pilon** https://github.com/broadinstitute/pilon/wiki
