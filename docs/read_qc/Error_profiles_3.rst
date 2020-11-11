
Generating Error Profiles(3)
-------------------------


Enhanced mapping statistics
^^^^^^^^^^^^^^^^^^^^^^^^^^^


We run::

  qualimap bamqc
  
with the following parameters:



+------------------------------------------+-------------------------+---------------------------------------------------------------------------+
| What?                                    | parameter               | Our value                                                                 |
+==========================================+=========================+===========================================================================+
| The input directory                      | -bam                    | ~/workdir/mappings/basecall_tiny_porechopped_<number>_vs_wuhan.sorted.bam |
|                                          |                         | or                                                                        |
|                                          |                         | ~/workdir/mappings/illumina_vs_wuhan.sorted.bam                           |
+------------------------------------------+-------------------------+---------------------------------------------------------------------------+ 
| The output directory                     | -outdir                 | ~/workdir/mappings/basecall_tiny_porechopped_<number>_vs_wuhan_qualimap/  |
|                                          |                         | or                                                                        |
|                                          |                         | ~/workdir/mappings/illumina_vs_wuhan_qualimap/                            |
+------------------------------------------+-------------------------+---------------------------------------------------------------------------+
| The number of threads to be used         | -nt                     | 14                                                                        |
+------------------------------------------+-------------------------+---------------------------------------------------------------------------+
| The number of windows                    | -nw                     | 400                                                                       |
+------------------------------------------+-------------------------+---------------------------------------------------------------------------+

The complete commandline is::

  


Then we can run **qualimap** on those BAM files now::
  
  qualimap bamqc -bam ~/workdir/map_to_ref/nanopore.graphmap.sorted.bam -nw 5000 -nt 14 -c -outdir ~/workdir/map_to_ref/nanopore.graphmap
  qualimap bamqc -bam ~/workdir/map_to_ref/illumina.bwa.sorted.bam -nw 5000 -nt 14 -c -outdir ~/workdir/map_to_ref/illumina.graphmap

Qualimap can also be run interactively.

References
^^^^^^^^^^

**QualiMap** http://qualimap.bioinfo.cipf.es/doc_html/index.html
