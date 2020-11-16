FastQC(2)
------

We need to run fastqc with the following command::

  fastqc
  
and with the following options:

+------------------------------------------+-------------------------+---------------------------------------------+
| What?                                    | parameter               | Our value                                   |
+==========================================+=========================+=============================================+
| The input directory                      | positional              | ~/workdir/data_artic/basecall_tiny_<number>/|
|                                          |                         | or                                          |
|                                          |                         | ~/workdir/Illumina/*.fastq.gz               |
+------------------------------------------+-------------------------+---------------------------------------------+ 
| The output directory                     | -o                      | ~/workdir/fastqc/basecall_tiny_<number>/    |
|                                          |                         | or                                          |
|                                          |                         | ~/workdir/fastqc/illumina/                  |
+------------------------------------------+-------------------------+---------------------------------------------+
| The number of threads to be used         | -t                      | 14                                          |
+------------------------------------------+-------------------------+---------------------------------------------+


Go to your workdir fist::

  cd ~/workdir
  
Then create folders for the results (fastqc will not create them)::

  mkdir -p ~/workdir/fastqc/basecall_tiny_<number>
  mkdir -p ~/workdir/fastqc/illumina
  
The run fastqc for Illumina data::  

  fastqc -t 14 -o ~/workdir/fastqc/illumina ~/workdir/Illumina/*.fastq.gz

  fastqc -t 14 -o ~/workdir/fastqc/basecall_tiny_<number> ~/workdir/data_artic/basecall_tiny_<number>.fastq.gz

After that, you can load the reports in your web browser. Open a file browser, go to your workdir/fastqc/ directory and double click the html file.
Or use the command line::

  firefox ~/workdir/fastqc/illumina*.html

  firefox ~/workdir/fastqc/basecall_tiny_<number>/*.html


Look out for the differences between Illumina data und Nanopore data. Is there Addapter contamination in your read data?


You should also check out the `FastQC home page <http://www.bioinformatics.babraham.ac.uk/projects/fastqc/>`_ for examples
of reports including bad data.

**Note:** When you run fastqc without "-o", the results of fastqc are located in the directory that contains the reads.

TODO: Wird das hier noch gebraucht?? Porechop auf naechster Seite...

Handle adapter contamination
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

As we see some strange GC content at the 5' end of our nanopore reads, we can alter the way the plots are generated and turn off the grouping of reads into bins. Notice, this will generate very huge plots! To avoid this, we will first trim our reads to the first 100 base positions and do the analysis only on that::

  cd ~/workdir
  mkdir -p ~/workdir/fastqc/nanopore_fastqc_nogroup
  zcat ~/workdir/basecall/basecall.fastq.gz  | perl -ne '{chomp; if ($.%2) {print $_."\n"} else {print substr($_,0,100)."\n"} }' | gzip > ~/workdir/basecall/basecall_100.fastq.gz
  fastqc -t 14 -o ~/workdir/fastqc/nanopore_fastqc_nogroup --nogroup --extract ~/workdir/basecall/basecall_100.fastq.gz
  grep -A 100 "Per base sequence" ~/workdir/fastqc/nanopore_fastqc_nogroup/basecall_100_fastqc/fastqc_data.txt 



So the first bases may indicate an adaptor contamination. For workflows including de novo assembly refined with nanopolish or medaka adaptor trimming is not necessary, but in other workflow scenarios this can be important to do and good there are tools which can handle this, as e.g. **porechop**.

References
^^^^^^^^^^

**FastQC** https://www.bioinformatics.babraham.ac.uk/projects/fastqc/

