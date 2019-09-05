Call pilon
----------

In the next step, we call pilon with the mappings to polish our assembly::
  
  cd ~/workdir/
  mkdir pilon
  java -Xmx32G -jar ~/pilon-1.22.jar --genome ~/workdir/assembly/assembly.contigs.fasta --fix all --changes --frags ~/workdir/illumina_mapping/mapping.sorted.bam --threads 14 --output ~/workdir/pilon/pilon_round1 | tee ~/workdir/pilon/round1.pilon
  
You can repeat this for several rounds like this::

  bwa index ~/workdir/pilon/pilon_round1.fasta

  bwa mem -t 14 ~/workdir/pilon/pilon_round1.fasta ~/workdir/data/illumina/Illumina_R1.fastq.gz ~/workdir/data/illumina/Illumina_R2.fastq.gz | samtools view - -Sb | samtools sort - -@14 -o ~/workdir/illumina_mapping/mapping_pilon1.sorted.bam
  
  samtools index ~/workdir/illumina_mapping/mapping_pilon1.sorted.bam
  
  java -Xmx32G -jar ~/pilon-1.22.jar --genome ~/workdir/pilon/pilon_round1.fasta --fix all --changes --frags ~/workdir/illumina_mapping/mapping_pilon1.sorted.bam --threads 14 --output ~/workdir/pilon/pilon_round2 | tee ~/workdir/pilon/round2.pilon

You can inspect the ``Pilon_roundX.changes`` file to see if there are changes to the previous round.

We have precomputed some pilon rounds for you. You can copy those rounds from the precomputed Result directory to your workdir::

  cp -r ~/workdir/results/pilon/pilon_round{2..4}* ~/workdir/pilon/.


References
^^^^^^^^^^

**pilon** https://github.com/broadinstitute/pilon/wiki
