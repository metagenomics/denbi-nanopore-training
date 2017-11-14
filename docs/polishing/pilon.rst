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

We have prepared a set of Illumina data for you, which we are now using for polishing with pilon::

  ls -l ~/Illumina/
  
  total 726604
  -rw-r--r-- 1 ubuntu ubuntu       432 Nov  7 08:55 assembly_stats.txt
  -rw-r--r-- 1 ubuntu ubuntu  28924964 Nov  7 08:55 MP2.fastq.fwd
  -rw-r--r-- 1 ubuntu ubuntu  29457256 Nov  7 08:55 MP2.fastq.rev
  -rw-r--r-- 1 ubuntu ubuntu  15943849 Nov  7 08:55 MP2.fastq.uni
  -rw-r--r-- 1 ubuntu ubuntu 315258550 Nov  7 08:55 TSPf_R1.fastq.gz
  -rw-r--r-- 1 ubuntu ubuntu 354446667 Nov  7 08:55 TSPf_R2.fastq.gz

First we are mapping the Illumina reads to the largest contig of our assembly. We already created an index, so we can skip this step::
  
  #already done
  bwa index ~/canu_assembly/largestContig.fasta
  
Then we are mapping all reads to the contig. Note that we are shortening the process of creating a sorted and indexed bam file by piping the output of bwa directly to samtools, thereby avoiding temporary files::

  mkdir Illumina_mappings

  bwa mem -t 16 ~/largestContig.fasta ~/Illumina/TSPf_R1.fastq.gz ~//Illumina/TSPf_R2.fastq.gz | samtools sort --threads 16 -o ~/Illumina_mappings/WGS.bam
  samtools index ~/Illumina_mappings/WGS.bam
  
  bwa mem -t 16 ID.contigs.fasta ~/Illumina/MP2.fastq.fwd ~/Illumina/MP2.fastq.rev | samtools sort --threads 16 -o ~/Illumina_mappings/MP.bam
  samtools index ~/Illumina_mappings/MP.bam
  
In the next step, we call pilon with the mappings to polish our assembly::
  
  ~> java -Xmx32G -jar ~/pilon-1.22.jar --genome ~/largestContig.fasta --fix all --changes --frags ~/Illumina_mappings/WGS.bam --jumps ~/Illumina_mappings/MP.bam --threads 16 --output Pilon_round1 | tee round1.pilon
  
Repeat this for 3-4 rounds. 

Assembly evaluation with quast
------------------------------

TODO: ANPASSEN!

As usual, we are going to use quast for assembly evaluation::

  cd
  quast.py -t 16 -o ~/quast_nanopolished_assembly -R ~/Reference/CXERO_10272017.fna ~/polishedContig_small.fasta

QUAST generates HTML reports including a number of interactive graphics. To access these reports, copy the
quast directory to your `www` folder::

  cp -r quast_nanopolished_assembly ~/www/

You can load the reports in your web browser::

  http://YOUR_OPENSTACK_INSTANCE_IP/quast_nanopolished_assembly/summary/report.html

Compare to the previous results without polishing.
