FastQC
------

Get Illumina data
^^^^^^^^^^^^^^^^^

We have prepared some Illumina data for comparison with Nanopore data for you. Get the data with::
  
  cd ~/workdir
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/Illumina.tar.gz

Then unpack and remove the tar file::

  cd ~/workdir/
  tar -xzvf Illumina.tar.gz
  rm Illumina.tar.gz

Check, what's in the Illumina directory::

  ls -l ~/workdir/Illumina/
  total 109108
  -rw-r--r-- 1 ubuntu ubuntu 53544266 Nov  4 14:04 D0003_S3_L001_R1_001.fastq.gz
  -rw-r--r-- 1 ubuntu ubuntu 58178627 Nov  4 14:04 D0003_S3_L001_R2_001.fastq.gz

FastQC
^^^^^^


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

Your task is to run FastQC on the Illumina data and on your basecalled samples. Create a new directory for the results::

  mkdir ~/workdir/fastqc
  
The results should be stored in::

  ~/workdir/fastqc/illumina/

and::

  ~/workdir/fastqc/basecall_tiny_<number>


You need to create the result directories before runnnig FastQC, because FastQC will note create them.

If you are stuck, go to the next page to get further help.


References
^^^^^^^^^^

**FastQC** https://www.bioinformatics.babraham.ac.uk/projects/fastqc/
