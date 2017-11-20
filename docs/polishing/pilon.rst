Assembly polishing with pilon
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

  ls -l ~/Data/Illumina/
  
  total 726604
  -rw-r--r-- 1 ubuntu ubuntu       432 Nov  7 08:55 assembly_stats.txt
  -rw-r--r-- 1 ubuntu ubuntu  28924964 Nov  7 08:55 MP2.fastq.fwd
  -rw-r--r-- 1 ubuntu ubuntu  29457256 Nov  7 08:55 MP2.fastq.rev
  -rw-r--r-- 1 ubuntu ubuntu  15943849 Nov  7 08:55 MP2.fastq.uni
  -rw-r--r-- 1 ubuntu ubuntu 315258550 Nov  7 08:55 TSPf_R1.fastq.gz
  -rw-r--r-- 1 ubuntu ubuntu 354446667 Nov  7 08:55 TSPf_R2.fastq.gz

Extracting first contig from Assembly
-------------------------------------

The ``variants --consensus`` option of nanopolish, which we will use later, only works on one contig. We continue to work only with the largest contig. We can do this, because we know, that this contig covers the complete reference. We are extracting the first (and largest) contig from our assembly::

  samtools faidx ~/workdir/canu_assembly/canuAssembly.contigs.fasta tig00000001 > ~/workdir/canu_assembly/largestContig.fasta

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
  
In the next step, we call pilon with the mappings to polish our assembly::
  
  cd
  mkdir Pilon
  java -Xmx32G -jar ~/pilon-1.22.jar --genome ~/workdir/canu_assembly/largestContig.fasta --fix all --changes --frags ~/workdir/Illumina_mappings/WGS.sorted.bam --jumps ~/workdir/Illumina_mappings/MP.sorted.bam --threads 16 --output ~/Pilon/Pilon_round1 | tee ~/Pilon/round1.pilon
  
Repeat this for 3-4 rounds like this::

  bwa index ~/workdir/Pilon/Pilon_round1.fasta

  bwa mem -t 16 ~/workdir/Pilon/Pilon_round1.fasta ~/Data/Illumina/TSPf_R1.fastq.gz ~/Data/Illumina/TSPf_R2.fastq.gz | samtools view - -Sb | samtools sort - -@16 -o sorted > ~/workdir/Illumina_mappings/WGS.sorted.bam
  
  samtools index ~/workdir/Illumina_mappings/WGS.sorted.bam
  
  bwa mem -t 16 ~/workdir/Pilon/Pilon_round1.fasta ~/Data/Illumina/MP2.fastq.fwd ~/Data/Illumina/MP2.fastq.rev | samtools view - -Sb | samtools sort - -@16 -o sorted > ~/workdir/Illumina_mappings/MP.sorted.bam
  
  samtools index ~/workdir/Illumina_mappings/MP.sorted.bam
  
  java -Xmx32G -jar ~/pilon-1.22.jar --genome ~/workdir/Pilon/Pilon_round1.fasta --fix all --changes --frags ~/workdir/Illumina_mappings/WGS.sorted.bam --jumps ~/workdir/Illumina_mappings/MP.sorted.bam --threads 16 --output ~/workdir/Pilon/Pilon_round2 | tee ~/workdir/Pilon/round2.pilon
  ...

You can inspect the ``Pilon_roundX.changes`` file to see if there are changes to the previous round.


Assembly evaluation with quast
------------------------------

As usual, we are going to use quast for assembly evaluation::

  cd
  quast.py -t 16 -o ~/quast_pilon -R ~/Reference/CXERO_10272017.fna ~/workdir/Pilon/Pilon_round1.fasta
  #and so on for other pilon rounds

QUAST generates HTML reports including a number of interactive graphics. To access these reports, copy the
quast directory to your `www` folder::

  cp -r ~/workdir/quast_pilon ~/www/

You can load the reports in your web browser::

  http://YOUR_OPENSTACK_INSTANCE_IP/pilon_round1/summary/report.html

Compare to the previous results without polishing.
