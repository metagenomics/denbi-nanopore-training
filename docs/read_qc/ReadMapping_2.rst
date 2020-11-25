
Read mapping to a reference (2)
-------------------------

Mapping the Nanopore data with Minimap2
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We use::

  minimap2
  
to compute the mapping and add the following parameters:

+------------------------------------------+----------------+-------------------------------------------------------------------+
| What?                                    | parameter      | Our value                                                         |
+==========================================+================+===================================================================+
| The reference file                       | positional (1) | ~/workdir/wuhan.fasta                                             |
+------------------------------------------+----------------+-------------------------------------------------------------------+
| The input file                           | positional (2) | ~/workdir/data_artic/basecall_tiny_porechopped_<number>/.fastq.gz |
+------------------------------------------+----------------+-------------------------------------------------------------------+ 
| The output file                          | -o             | ~/workdir/mappings/basecall_tiny_porechopped_<number>_vs_wuhan.sam|
+------------------------------------------+----------------+-------------------------------------------------------------------+
| The number of threads to be used         | -t             | 14                                                                |
+------------------------------------------+----------------+-------------------------------------------------------------------+
| The preset for Nanopore reads            | -x             | map-ont                                                           |
+------------------------------------------+----------------+-------------------------------------------------------------------+
| Get sam output                           | -a                                                                                 |
+------------------------------------------+----------------+-------------------------------------------------------------------+

Create the mapping folder first::

  mkdir ~/workdir/mappings/

The complete commandline for minimap2 is::

  minimap2 -t 14 -x map-ont -a -o ~/workdir/mappings/basecall_tiny_porechopped_<number>_vs_wuhan.sam ~/workdir/wuhan.fasta ~/workdir/data_artic/basecall_tiny_porechopped_<number>/.fastq.gz


Mapping the Illumina data with bwa
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

First, we use::

  bwa index ~/workdir/wuhan.fasta
  
to create an index on the reference.

Then, we run the actual mapping with::

  bwa mem
  
and the following parameters:

+------------------------------------------+----------------+-------------------------------------------------------------------+
| What?                                    | parameter      | Our value                                                         |
+==========================================+================+===================================================================+
| The reference file                       | positional (1) | ~/workdir/wuhan.fasta                                             |
+------------------------------------------+----------------+-------------------------------------------------------------------+
| The input file(s)                        | positional (2) | ~/workdir/Illumina/D0003_S3_L001_R1_001.fastq.gz                  |
|                                          |                | and                                                               |
|                                          |                | ~/workdir/illumina/D0003_S3_L001_R2_001.fastq.gz                  |
+------------------------------------------+----------------+-------------------------------------------------------------------+ 
| The output file                          | redirect with >| ~/workdir/mappings/illumina_vs_wuhan.sam                          |
+------------------------------------------+----------------+-------------------------------------------------------------------+
| The number of threads to be used         | -t             | 14                                                                |
+------------------------------------------+----------------+-------------------------------------------------------------------+


The complete commandline for bwa mem is::

  bwa mem -t 14  ~/workdir/wuhan.fasta  ~/workdir/Illumina/D0003_S3_L001_R1_001.fastq.gz ~/workdir/Illumina/D0003_S3_L001_R2_001.fastq.gz > ~/workdir/mappings/illumina_vs_wuhan.sam


Converting sam files to sorted and indexed bam files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In order to read the mapping files into a genome browser, we need to convert the .sam files to sorted .bam files. This is done, using 3 different tools from samtools, the first converts sam to bam::

  Usage: samtools view [options] <in.bam>|<in.sam>|<in.cram> [region ...]

  Options:
    -b       output BAM
    -C       output CRAM (requires -T)
    -1       use fast BAM compression (implies -b)
    -u       uncompressed BAM output (implies -b)
    -h       include header in SAM output
    -H       print SAM header only (no alignments)
    -c       print only the count of matching records
    -o FILE  output file name [stdout]
    -U FILE  output reads not selected by filters to FILE [null]
    -t FILE  FILE listing reference names and lengths (see long help) [null]
    -X       include customized index file
    -L FILE  only include reads overlapping this BED FILE [null]
    -r STR   only include reads in read group STR [null]
    -R FILE  only include reads with read group listed in FILE [null]
    -d STR:STR
             only include reads with tag STR and associated value STR [null]
    -D STR:FILE
             only include reads with tag STR and associated values listed in
             FILE [null]
    -q INT   only include reads with mapping quality >= INT [0]
    -l STR   only include reads in library STR [null]
    -m INT   only include reads with number of CIGAR operations consuming
             query sequence >= INT [0]
    -f INT   only include reads with all  of the FLAGs in INT present [0]
    -F INT   only include reads with none of the FLAGS in INT present [0]
    -G INT   only EXCLUDE reads with all  of the FLAGs in INT present [0]
    -s FLOAT subsample reads (given INT.FRAC option value, 0.FRAC is the
             fraction of templates/read pairs to keep; INT part sets seed)
    -M       use the multi-region iterator (increases the speed, removes
             duplicates and outputs the reads as they are ordered in the file)
    -x STR   read tag to strip (repeatable) [null]
    -B       collapse the backward CIGAR operation
    -?       print long help, including note about region specification
    -S       ignored (input format is auto-detected)
    --no-PG  do not add a PG line
        --input-fmt-option OPT[=VAL]
                 Specify a single input file format option in the form
                 of OPTION or OPTION=VALUE
    -O, --output-fmt FORMAT[,OPT[=VAL]]...
                 Specify output format (SAM, BAM, CRAM)
        --output-fmt-option OPT[=VAL]
                 Specify a single output file format option in the form
                 of OPTION or OPTION=VALUE
    -T, --reference FILE
                 Reference sequence FASTA FILE [null]
    -@, --threads INT
                 Number of additional threads to use [0]
        --write-index
                 Automatically index the output files [off]
        --verbosity INT
                 Set level of verbosity

For our purpose, we need the options::

  -b for sam to bam conversion

Redirect the output into files with name (or redirect directly to samtools sort - see further below)::

  ~/workdir/mappings/basecall_tiny_porechopped_<number>_vs_wuhan.bam
  or 
  ~/workdir/mappings/illumina_vs_wuhan.bam
  
  
Then sort the bam file with samtools sort::

  Usage: samtools sort [options...] [in.bam]
  Options:
    -l INT     Set compression level, from 0 (uncompressed) to 9 (best)
    -u         Output uncompressed data (equivalent to -l 0)
    -m INT     Set maximum memory per thread; suffix K/M/G recognized [768M]
    -M         Use minimiser for clustering unaligned/unplaced reads
    -K INT     Kmer size to use for minimiser [20]
    -n         Sort by read name (not compatible with samtools index command)
    -t TAG     Sort by value of TAG. Uses position as secondary index (or read name if -n is set)
    -o FILE    Write final output to FILE rather than standard output
    -T PREFIX  Write temporary files to PREFIX.nnnn.bam
    --no-PG    do not add a PG line
        --input-fmt-option OPT[=VAL]
                 Specify a single input file format option in the form
                 of OPTION or OPTION=VALUE
    -O, --output-fmt FORMAT[,OPT[=VAL]]...
                 Specify output format (SAM, BAM, CRAM)
        --output-fmt-option OPT[=VAL]
                 Specify a single output file format option in the form
                 of OPTION or OPTION=VALUE
        --reference FILE
                 Reference sequence FASTA FILE [null]
    -@, --threads INT
                 Number of additional threads to use [0]
        --verbosity INT
                 Set level of verbosity

Your sorted bam files should have the name::

  ~/workdir/mappings/basecall_tiny_porechopped_<number>_vs_wuhan.sorted.bam
  or 
  ~/workdir/mappings/illumina_vs_wuhan.sorted.bam

Then index the sorted bam file::

  Usage: samtools index [-bc] [-m INT] <in.bam> [out.index]
  Options:
    -b       Generate BAI-format index for BAM files [default]
    -c       Generate CSI-format index for BAM files
    -m INT   Set minimum interval size for CSI indices to 2^INT [14]
    -@ INT   Sets the number of threads [none]

If you are stuck - get help on the next page.


References
^^^^^^^^^^

**Minimap2** https://github.com/lh3/minimap2

**BWA** http://bio-bwa.sourceforge.net/

**samtools** http://www.htslib.org
