Assembly polishing with nanopolish
==================================

Nanopolish is a software package for signal-level analysis of Oxford Nanopore sequencing data. Nanopolish can calculate an improved consensus sequence for a draft genome assembly, detect base modifications, call SNPs and indels with respect to a reference genome and more. The following modules are available::

  nanopolish extract: extract reads in FASTA or FASTQ format from a directory of FAST5 files
  nanopolish call-methylation: predict genomic bases that may be methylated
  nanopolish variants: detect SNPs and indels with respect to a reference genome
  nanopolish variants --consensus: calculate an improved consensus sequence for a draft genome assembly
  nanopolish eventalign: align signal-level events to k-mers of a reference genome

.. toctree::
   :maxdepth: 1

   mapping
   indexing
   nanopolish
   quast


References
^^^^^^^^^^

**nanopolish** https://github.com/jts/nanopolish
