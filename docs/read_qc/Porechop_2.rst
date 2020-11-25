Porechop(2)
------

We need to run the following command::

  porechop
  
with the following parameters

+------------------------------------------+----------------+-------------------------------------------------------------------+
| What?                                    | parameter      | Our value                                                         |
+==========================================+================+===================================================================+
| The input file                           | -i             | ~/workdir/data_artic/basecall_tiny_<number>/.fastq.gz             |
+------------------------------------------+----------------+-------------------------------------------------------------------+ 
| The output file                          | -o             | ~/workdir/data_artic/basecall_tiny_porechopped_<number>/.fastq.gz |
+------------------------------------------+----------------+-------------------------------------------------------------------+
| The number of threads to be used         | -t             | 14                                                                |
+------------------------------------------+----------------+-------------------------------------------------------------------+
| The verbosity                            | -v             | 2                                                                 |
+------------------------------------------+----------------+-------------------------------------------------------------------+

Go to your data_artic directory first::

  cd ~/workdir/data_artic/

Then run porechop::

  porechop -i ~/workdir/data_artic/basecall_tiny_<number>.fastq.gz -t 14 -v 2 -o ~/workdir/data_artic/basecall_tiny_porechopped_<number>.fastq.gz > ~/workdir/data_artic/porechop_<number>.log


**Task**: Inspect the log file -have adapters been removed?

And finally, run FastQC again, to compare with the untrimmed data:: 

  mkdir ~/workdir/fastqc/basecall_tiny_trimmed_<number>/
  fastqc -t 14 -o  ~/workdir/fastqc/basecall_tiny_trimmed_<number> ~/workdir/data_artic/basecall_tiny_porechopped_<number>.fastq.gz


References
^^^^^^^^^^

**FastQC** https://www.bioinformatics.babraham.ac.uk/projects/fastqc/

**Porechop** https://github.com/rrwick/Porechop
