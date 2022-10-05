Mapping of Illumina reads to assembly 
-------------------------------------

We are mapping the Illumina reads our consensus sequence with BWA. BWA is a software package for mapping low-divergent sequences against a large reference genome. It consists of three algorithms: BWA-backtrack, BWA-SW and BWA-MEM. The first algorithm is designed for Illumina sequence reads up to 100bp, while the rest two for longer sequences ranged from 70bp to 1Mbp. BWA-MEM and BWA-SW share similar features such as long-read support and split alignment, but BWA-MEM, which is the latest, is generally recommended for high-quality queries as it is faster and more accurate.


Mapping the Illumina data with bwa
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

First, we need to create an index on the consensus sequences::

  bwa index ~/workdir/results_artic/barcode_01.consensus.fasta
  

Then, we run the actual mapping with::

  bwa mem
  
and the following parameters:

+------------------------------------------+----------------+------------------------------------------------------------------------------------+
| What?                                    | parameter      | Our value                                                                          |
+==========================================+================+====================================================================================+
| The reference file                       | positional (1) | ~/workdir/results_artic/barcode_01.consensus.fasta                           |
+------------------------------------------+----------------+------------------------------------------------------------------------------------+
| The input file(s)                        | positional (2) | ~/workdir/data_artic/Illumina_01/B0001_S1_L001_R1_001.fastq.gz |
|                                          |                | and                                                                                |
|                                          |                | ~/workdir/data_artic/Illumina_01/B0001_S1_L001_R2_001.fastq.gz |
+------------------------------------------+----------------+------------------------------------------------------------------------------------+ 
| The output file                          | redirect with >| ~/workdir/mappings/Illumina_vs_consensus_01.sam                              |
+------------------------------------------+----------------+------------------------------------------------------------------------------------+
| The number of threads to be used         | -t             | 14                                                                                 |
+------------------------------------------+----------------+------------------------------------------------------------------------------------+

**Note:** You don't need to create a SAM file, when you pipe the results of ``bwa mem`` directly to ``samtools`` for SAM to BAM conversion and sorting::

  <bwa command> | samtools view -b - | samtools sort - > ~/workdir/mappings/Illumina_vs_consensus_01.sorted.bam

The complete commandline for bwa mem is::

  bwa mem -t 14  ~/workdir/results_artic/barcode_01.consensus.fasta  ~/workdir/data_artic/Illumina_01/B0001_S1_L001_R1_001.fastq.gz ~/workdir/data_artic/Illumina_01/B0001_S1_L001_R2_001.fastq.gz > ~/workdir/mappings/Illumina_vs_consensus_01.sam
  
Then convert to BAM (if you didn't created a sorted BAM file directly)::

  samtools view -b ~/workdir/mappings/Illumina_vs_consensus_01.sam | samtools sort - > ~/workdir/mappings/Illumina_vs_consensus_01.sorted.bam
  
And finally, no matter how you created the sorted BAM file, create an index on it::

  samtools index ~/workdir/mappings/Illumina_vs_consensus_01.sorted.bam

**Perform that step for the first (01) dataset only to save time. Do the other datasets later, when there is time left.**


References
^^^^^^^^^^

**BWA** http://bio-bwa.sourceforge.net/

**samtools** http://www.htslib.org
