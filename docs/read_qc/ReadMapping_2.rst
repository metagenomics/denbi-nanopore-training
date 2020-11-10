
Read mapping to a reference (2)
-------------------------

Mapping the Nanopore data with Minimap2
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We use::

  minimap2
  
to compute the mapping and add the following parameters

+------------------------------------------+----------------+-------------------------------------------------------------------+
| What?                                    | parameter      | Our value                                                         |
+==========================================+================+===================================================================+
| The reference file                       | positional (1) | ~/workdir/wuhan.fasta |
+------------------------------------------+----------------+-------------------------------------------------------------------+
| The input file                           | positional (2) | ~/workdir/data_artic/basecall_tiny_porechopped_<number>/.fastq.gz |
+------------------------------------------+----------------+-------------------------------------------------------------------+ 
| The output file                          | -o             | ~/workdir/mappings/basecall_tiny_porechopped_<number>_vs_wuhan.sam|
+------------------------------------------+----------------+-------------------------------------------------------------------+
| The number of threads to be used         | -t             | 14                                                                |
+------------------------------------------+----------------+-------------------------------------------------------------------+
| The preset for Nanopore reads            | -x             | map-ont                                                           |
+------------------------------------------+----------------+-------------------------------------------------------------------+

The complete commandline for minimap2 is::

  minimap2 -t 14 -x map-ont -o ~/workdir/mappings/basecall_tiny_porechopped_<number>_vs_wuhan.sam ~/workdir/wuhan.fasta ~/workdir/data_artic/basecall_tiny_porechopped_<number>/.fastq.gz


References
^^^^^^^^^^

**Minimap2** https://github.com/lh3/minimap2

**BWA** http://bio-bwa.sourceforge.net/
