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
