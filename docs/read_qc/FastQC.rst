FastQC
------

FastQC aims to provide a simple way to do some quality control checks
on raw sequence data coming from high throughput sequencing
pipelines. It provides a modular set of analyses which you can use to
give a quick impression of whether your data has any problems of which
you should be aware before doing any further analysis.

The main functions of FastQC are

* Import of data from BAM, SAM or FastQ files (any variant)
* Providing a quick overview to tell you in which areas there may be problems
* Summary graphs and tables to quickly assess your data
* Export of results to an HTML based permanent report
* Offline operation to allow automated generation of reports without running the interactive application

You can run FastQC interactively or using ht CLI, which offers the following options::

  fastqc --help
            
  SYNOPSIS

    fastqc seqfile1 seqfile2 .. seqfileN

    fastqc [-o output dir] [--(no)extract] [-f fastq|bam|sam] [-c contaminant file] seqfile1 .. seqfileN

  OPTIONS:

    -o --outdir     Create all output files in the specified output directory.
                    Please note that this directory must exist as the program
                    will not create it.  If this option is not set then the 
                    output file for each sequence file is created in the same
                    directory as the sequence file which was processed.
                    
    --casava        Files come from raw casava output. Files in the same sample
                    group (differing only by the group number) will be analysed
                    as a set rather than individually. Sequences with the filter
                    flag set in the header will be excluded from the analysis.
                    Files must have the same names given to them by casava
                    (including being gzipped and ending with .gz) otherwise they
                    won't be grouped together correctly.
                    
    --nano          Files come from naopore sequences and are in fast5 format. In
                    this mode you can pass in directories to process and the program
                    will take in all fast5 files within those directories and produce
                    a single output file from the sequences found in all files.                    
                    
    --nofilter      If running with --casava then don't remove read flagged by
                    casava as poor quality when performing the QC analysis.
                        
    --nogroup       Disable grouping of bases for reads >50bp. All reports will
                    show data for every base in the read.  WARNING: Using this
                    option will cause fastqc to crash and burn if you use it on
                    really long reads, and your plots may end up a ridiculous size.
                    You have been warned!
                    
    -f --format     Bypasses the normal sequence file format detection and
                    forces the program to use the specified format.  Valid
                    formats are bam,sam,bam_mapped,sam_mapped and fastq
                    
    -t --threads    Specifies the number of files which can be processed
                    simultaneously.  Each thread will be allocated 250MB of
                    memory so you shouldn't run more threads than your
                    available memory will cope with, and not more than
                    6 threads on a 32 bit machine
                  
    -c              Specifies a non-default file which contains the list of
    --contaminants  contaminants to screen overrepresented sequences against.
                    The file must contain sets of named contaminants in the
                    form name[tab]sequence.  Lines prefixed with a hash will
                    be ignored.

    -a              Specifies a non-default file which contains the list of
    --adapters      adapter sequences which will be explicity searched against
                    the library. The file must contain sets of named adapters
                    in the form name[tab]sequence.  Lines prefixed with a hash
                    will be ignored.
                    
    -l              Specifies a non-default file which contains a set of criteria
    --limits        which will be used to determine the warn/error limits for the
                    various modules.  This file can also be used to selectively 
                    remove some modules from the output all together.  The format
                    needs to mirror the default limits.txt file found in the
                    Configuration folder.
                    
   -k --kmers       Specifies the length of Kmer to look for in the Kmer content
                    module. Specified Kmer length must be between 2 and 10. Default
                    length is 7 if not specified.
                    
   -q --quiet       Supress all progress messages on stdout and only report errors.
   
   -d --dir         Selects a directory to be used for temporary files written when
                    generating report images. Defaults to system temp directory if
                    not specified.

See the `FastQC home page <http://www.bioinformatics.babraham.ac.uk/projects/fastqc/>`_ for more info.

QA with FastQC
^^^^^^^^^^^^^^
Evaluate with fastqc::
  
  cd ~/workdir
  mkdir -p ~/workdir/FastQC/1D_fastqc
  mkdir -p ~/workdir/FastQC/1D2_fastqc
  mkdir -p ~/workdir/FastQC/illumina_fastqc
  fastqc -t 14 -o ~/workdir/FastQC/1D_fastqc/ 1D_basecall.fastq
  fastqc -t 14 -o ~/workdir/FastQC/1D2_fastqc/ 1D2_basecall.fastq
  fastqc -t 14 -o ~/workdir/FastQC/illumina_fastqc/ ~/workdir/Data/Illumina/TSPf_R1.fastq.gz ~/workdir/Data/Illumina/TSPf_R2.fastq.gz
  
After that, you can load the reports in your web browser via Cloud9. Just right-click on the file in the
directory tree on the left side of your Cloud9 window and choose "Preview".
  
We will inspect the results together now ...

You should also check out the `FastQC home page <http://www.bioinformatics.babraham.ac.uk/projects/fastqc/>`_ for examples
of reports including bad data.

Handle adapter contamination
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

As we see some strange GC content at the 5' end of our nanopore reads, we can alter the way the plots are generated and turn off the grouping of reads into bins. Notice, this will generate very huge plots! To avoid this, we will first trim our reads to the first 100 base positions and do the analysis only on that::

  cd ~/workdir
  mkdir -p ~/workdir/FastQC/1D_fastqc_nogroup
  cat ~/workdir/1D_basecall.fastq  | perl -ne '{chomp; if ($_ =~ m/^@.{8}-.{4}-.{4}-.{12}/) {print $_."\n"} else {print substr($_,0,100)."\n"} }' > ~/workdir/1D_basecall_100.fastq
  fastqc -t 14 -o ~/workdir/FastQC/1D_fastqc_nogroup/ --nogroup --extract 1D_basecall_100.fastq  
  grep -A 100 "Per base sequence" ~/workdir/FastQC/1D_fastqc_nogroup/1D_basecall_100_fastqc/fastqc_data.txt 
  
So the first bases may indicate an adaptor contamination. For workflows including de novo assembly refined with nanopolish adaptor trimming is not necessary, but in other workflow scenarios this can be important to do and good there are tools which can handle this, as e.g. **porechop**.

Porechop is a tool for finding and removing adapters from Oxford Nanopore reads. Adapters on the ends of reads are trimmed off, and when a read has an adapter in its middle, it is treated as chimeric and chopped into separate reads. Porechop performs thorough alignments to effectively find adapters, even at low sequence identity::

  cd ~/workdir
  porechop -i 1D_basecall.fastq -t 14 -v 2 -o 1D_basecall.trimmed.fastq > porechop.log

Let's inspect the log file::

  cat porechop.log | more
  
So here, the following adapters were found and trimmed of in 16,500 of 20,051 cases::

  SQK-NSK007_Y_Top:     AATGTACTTCGTTCAGTTACGTATTGCT
  SQK-NSK007_Y_Bottom:  GCAATACGTAACTGAACGAAGT
  1D2_part_1_start:     GAGAGGTTCCAAGTCAGAGAGGTTCCT
  1D2_part_1_end:       AGGAACCTCTCTGACTTGGAACCTCTC
  1D2_part_2_start:     CTTCGTTCAGTTACGTATTGCTGGCGTCTGCTT
  1D2_part_2_end:       CACCCAAGCAGACGCCAGCAATACGTAACT


We will again look into the results of FastQC::

  mkdir -p ~/workdir/FastQC/1D_fastqc_trimmed
  fastqc -t 14 -o ~/workdir/FastQC/1D_fastqc_trimmed/ 1D_basecall.trimmed.fastq
  
References
^^^^^^^^^^

**FastQC** https://www.bioinformatics.babraham.ac.uk/projects/fastqc/

**Porechop** https://github.com/rrwick/Porechop
