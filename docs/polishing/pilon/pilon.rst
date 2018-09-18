Call pilon
----------

In the next step, we call pilon with the mappings to polish our assembly::
  
  cd ~/workdir/
  mkdir Pilon
  java -Xmx32G -jar ~/pilon-1.22.jar --genome ~/workdir/canu_assembly/largestContig.fasta --fix all --changes --frags ~/workdir/Illumina_mappings/WGS.sorted.bam --jumps ~/workdir/Illumina_mappings/MP.sorted.bam --threads 16 --output ~/workdir/Pilon/Pilon_round1 | tee ~/workdir/Pilon/round1.pilon
  
You can repeat this for several rounds like this::

  bwa index ~/workdir/Pilon/Pilon_round1.fasta

  bwa mem -t 14 ~/workdir/Pilon/Pilon_round1.fasta ~/workdir/Data/Illumina/TSPf_R1.fastq.gz ~/workdir/Data/Illumina/TSPf_R2.fastq.gz | samtools view - -Sb | samtools sort - -@14 -o ~/workdir/Illumina_mappings/WGS.sorted.bam
  
  samtools index ~/workdir/Illumina_mappings/WGS.sorted.bam
  
  bwa mem -t 14 ~/workdir/Pilon/Pilon_round1.fasta ~/workdir/Data/Illumina/MP2.fastq.fwd ~/workdir/Data/Illumina/MP2.fastq.rev | samtools view - -Sb | samtools sort - -@14 -o ~/workdir/Illumina_mappings/MP.sorted.bam
  
  samtools index ~/workdir/Illumina_mappings/MP.sorted.bam
  
  java -Xmx32G -jar ~/pilon-1.22.jar --genome ~/workdir/Pilon/Pilon_round1.fasta --fix all --changes --frags ~/workdir/Illumina_mappings/WGS.sorted.bam --jumps ~/workdir/Illumina_mappings/MP.sorted.bam --threads 14 --output ~/workdir/Pilon/Pilon_round2 | tee ~/workdir/Pilon/round2.pilon
  ...

You can inspect the ``Pilon_roundX.changes`` file to see if there are changes to the previous round.

We have precomputed some pilon rounds for you. You can copy those rounds from the precomputed Result directory to your workdir::

  cp -r ~/workdir/Results/Pilon/Pilon_round{2..4}* ~/workdir/Pilon/.


References
^^^^^^^^^^

**pilon** https://github.com/broadinstitute/pilon/wiki
