Assembly polishing with racon and medaka
=============================

Pilon is a software tool which can be used to automatically improve draft assemblies or to find variation among strains, including large event detection.
Pilon requires as input a FASTA file of the genome along with one or more BAM files of reads aligned to the input FASTA file. Pilon uses read alignment analysis to identify inconsistencies between the input genome and the evidence in the reads. It then attempts to make improvements to the input genome, including:

- Single base differences
- Small indels
- Larger indel or block substitution events
- Gap filling
- Identification of local misassemblies, including optional opening of new gaps

Pilon then outputs a FASTA file containing an improved representation of the genome from the read data and an optional VCF file detailing variation seen between the read data and the input genome.

To aid manual inspection and improvement by an analyst, Pilon can optionally produce tracks that can be displayed in genome viewers such as IGV and GenomeView, and it reports other events (such as possible large collapsed repeat regions) in its standard output.
More information on pilon can be found here:
https://github.com/broadinstitute/pilon/wiki


We have prepared a set of Illumina data for you, which we are now using for polishing with pilon::

  ls -l ~/workdir/data/illumina/
  
  total 164068
  -rw-r--r-- 1 ubuntu ubuntu 78277075 Aug 30 08:20 Illumina_R1.fastq.gz
  -rw-r--r-- 1 ubuntu ubuntu 89724312 Aug 30 08:20 Illumina_R2.fastq.gz

.. toctree::
 :maxdepth: 1

 racon
 medaka
 quast
 pilon



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

  
  
  
  
  


