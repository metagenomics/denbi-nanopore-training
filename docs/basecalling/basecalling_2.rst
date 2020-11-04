Basecalling with Guppy
-------------------------

We need to specify the following options:

+------------------------------------------------------------------------+-------------------------+---------------------------------------------+
| What?                                                                  | parameter               | Our value                                   |
+========================================================================+=========================+=============================================+
| The config file for our flowcell/kit combination                       | -c                      | dna_r9.4.1_450bps_hac_model.cfg             |
+------------------------------------------------------------------------+-------------------------+---------------------------------------------+ 
| Compress the fastq output                                              | --compress_fastq                                                      |
+------------------------------------------------------------------------+-------------------------+---------------------------------------------+
| The full path to the directory where the raw read files are located    | -i                      | ~/workdir/data_artic/barcode_tiny_01/       |
+------------------------------------------------------------------------+-------------------------+---------------------------------------------+
| The full path to the directory where the basecalled files will be saved| -s                      | ~/workdir/data_artic/basecall_tiny_01/      |
+------------------------------------------------------------------------+-------------------------+---------------------------------------------+
| How many worker threads you are using                                  | --cpu_threads_per_caller| 14                                          |
+------------------------------------------------------------------------+-------------------------+---------------------------------------------+
| Number of parallel basecallers to create                               | --num_callers           | 1                                           |
+------------------------------------------------------------------------+-------------------------+---------------------------------------------+


Our complete command line is::

  guppy_basecaller_cpu --compress_fastq -i ~/workdir/data/fast5_tiny/ -s ~/workdir/basecall_tiny/ --cpu_threads_per_caller 14 --num_callers 1 -c dna_r9.4.1_450bps_hac.cfg
 
 
References
^^^^^^^^^^

**guppy** https://nanoporetech.com/
