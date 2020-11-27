FastQC(2)
------

We need to run fastqc with the following command::

  fastqc
  
and with the following options:

+------------------------------------------+-------------------------+---------------------------------------------+
| What?                                    | parameter               | Our value                                   |
+==========================================+=========================+=============================================+
| The input directory                      | positional              | ~/workdir/data_artic/basecall_tiny/         |
|                                          |                         | or                                          |
|                                          |                         | ~/workdir/Illumina/*.fastq.gz               |
+------------------------------------------+-------------------------+---------------------------------------------+ 
| The output directory                     | -o                      | ~/workdir/fastqc/basecall_tiny/             |
|                                          |                         | or                                          |
|                                          |                         | ~/workdir/fastqc/illumina/                  |
+------------------------------------------+-------------------------+---------------------------------------------+
| The number of threads to be used         | -t                      | 14                                          |
+------------------------------------------+-------------------------+---------------------------------------------+


Go to your workdir fist::

  cd ~/workdir
  
Then create folders for the results (fastqc will not create them)::

  mkdir -p ~/workdir/fastqc/basecall_tiny
  mkdir -p ~/workdir/fastqc/illumina
  
The run fastqc for Illumina data::  

  fastqc -t 14 -o ~/workdir/fastqc/illumina ~/workdir/Illumina/*.fastq.gz

  fastqc -t 14 -o ~/workdir/fastqc/basecall_tiny ~/workdir/data_artic/basecall_tiny.fastq.gz

After that, you can load the reports in your web browser. Open a file browser, go to your workdir/fastqc/ directory and double click the html file.
Or use the command line::

  firefox ~/workdir/fastqc/illumina*.html

  firefox ~/workdir/fastqc/basecall_tiny/*.html


Look out for the differences between Illumina data und Nanopore data. Is there Addapter contamination in your read data?


You should also check out the `FastQC home page <http://www.bioinformatics.babraham.ac.uk/projects/fastqc/>`_ for examples
of reports including bad data.

**Note:** When you run fastqc without "-o", the results of fastqc are located in the directory that contains the reads.

So the first bases may indicate an adapter contamination. For workflows including *de novo* assembly refined with nanopolish or medaka adapter trimming is not necessary, but in other workflow scenarios this can be important to do and good there are tools which can handle this, as e.g. **porechop**.

References
^^^^^^^^^^

**FastQC** https://www.bioinformatics.babraham.ac.uk/projects/fastqc/

