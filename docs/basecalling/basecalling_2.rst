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
| The full path to the directory where the raw read files are located    | -i                      | ~/workdir/data_artic/barcode_tiny_<number>/ |
+------------------------------------------------------------------------+-------------------------+---------------------------------------------+
| The full path to the directory where the basecalled files will be saved| -s                      | ~/workdir/data_artic/basecall_tiny_<number>/|
+------------------------------------------------------------------------+-------------------------+---------------------------------------------+
| How many worker threads you are using                                  | --cpu_threads_per_caller| 14                                          |
+------------------------------------------------------------------------+-------------------------+---------------------------------------------+
| Number of parallel basecallers to create                               | --num_callers           | 1                                           |
+------------------------------------------------------------------------+-------------------------+---------------------------------------------+




Our complete command line using these parameters is::

  guppy_basecaller_cpu --compress_fastq -i  ~/workdir/data_artic/barcode_tiny_<number>/ -s ~/workdir/data_artic/basecall_tiny_<number>/ --cpu_threads_per_caller 14 --num_callers 1 -c dna_r9.4.1_450bps_hac.cfg
 
 
 
**Repeat the basecalling for the remaining samples.**
 
References
^^^^^^^^^^

**guppy** https://nanoporetech.com/
