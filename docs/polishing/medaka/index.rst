racon first::
  
  cd ~/workdir/nanopore_mapping
  samtools view -S mapping.sorted.bam > mapping.sorted.sam
  cd ~/workdir/
  mkdir racon
  racon -t 14 ~/workdir/basecall/basecall.fastq.gz ~/workdir/nanopore_mapping/mapping.sorted.sam ~/workdir/assembly/assembly.contigs.fasta > racon/racon.fasta
  


medaka is a tool to create a consensus sequence of nanopore sequencing data. This task is performed using neural networks applied a pileup of individual sequencing reads against a draft assembly. It outperforms graph-based methods operating on basecalled data, and can be competitive with state-of-the-art signal-based methods whilst being much faster.

As input medaka accepts reads in either a .fasta or a .fastq file. It requires a draft assembly as a .fasta.

medaka usage::

  medaka_consensus [-h] -i <fastx>

    -h  show this help text.
    -i  fastx input basecalls (required).
    -d  fasta input assembly (required). 
    -o  output folder (default: medaka).
    -m  medaka model, (default: r941_min_high).
        Available: r941_trans, r941_flip213, r941_flip235, r941_min_fast, r941_min_high, r941_prom_fast, r941_prom_high.
        Alternatively a .hdf file from 'medaka train'. 
    -t  number of threads with which to create features (default: 1).
    -b  batchsize, controls memory use (default: 200).

  -i must be specified.


medaka commandline::

  medaka_consensus -i basecall/basecall.fastq.gz -d assembly/assembly.contigs.fasta -o medaka -t 14 -m r941_min_high
  
medaka after racon::

  medaka_consensus -i basecall/basecall.fastq.gz -d racon/racon.fasta -o racon_medaka -t 14 -m r941_min_high
  
  
pilon after medaka::

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

  
  
  
  
  


