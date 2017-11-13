Assembly polishing with nanopolish
==================================

Nanopolish is a software package for signal-level analysis of Oxford Nanopore sequencing data. Nanopolish can calculate an improved consensus sequence for a draft genome assembly, detect base modifications, call SNPs and indels with respect to a reference genome and more. The following modules are available::

  nanopolish extract: extract reads in FASTA or FASTQ format from a directory of FAST5 files
  nanopolish call-methylation: predict genomic bases that may be methylated
  nanopolish variants: detect SNPs and indels with respect to a reference genome
  nanopolish variants --consensus: calculate an improved consensus sequence for a draft genome assembly
  nanopolish eventalign: align signal-level events to k-mers of a reference genome



Extracting first contig from Assembly
-------------------------------------

::

  samtools faidx ...

Mapping of reads to assembly
----------------------------

TODO: Check this and change to one contig only::

  mkdir canu_assembly_with_index
  cp canu_assembly/1.contigs.fasta canu_assembly_with_index/
  cd canu_assembly_with_index/
  bwa index 1.contigs.fasta
  cd ..
  bwa mem -t 16 -k11 -W17 -r10 -A1 -B1 -O1 -E1 -L0 canu_assembly_with_index/1.contigs.fasta 1D2_Nanopolish/1D2_Nanopolish.fastq
  samtools view -Sb Mapping_Nanopore_to_assembly.sam > Mapping_Nanopore_to_assembly.bam
  samtools sort -@16 Mapping_Nanopore_to_assembly.bam Mapping_Nanopore_to_assembly.sorted
  samtools index Mapping_Nanopore_to_assembly.sorted.bam


Creating fastq from 1D^2 Basecalling
------------------------------------

Alt::
  # Only run this if you used Albacore 1.2 or later
  nanopolish extract -q -r -o 1D2Nanopolish/1D2_Nanopolish.fastq D1_2_basecall/workspace/

Wg. Albacore > 2.0::
  # Only run this if you used Albacore 2.0 or later
  nanopolish index -d /path/to/raw_fast5s/ albacore_output.fastq
  
 
