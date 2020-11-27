
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
| The input directory                      | -bam                    | ~/workdir/mappings/basecall_tiny_porechopped_vs_wuhan.sorted.bam |
|                                          |                         | or                                                                        |
|                                          |                         | ~/workdir/mappings/illumina_vs_wuhan.sorted.bam                           |
+------------------------------------------+-------------------------+---------------------------------------------------------------------------+ 
| The output directory                     | -outdir                 | ~/workdir/mappings/basecall_tiny_porechopped_vs_wuhan_qualimap/  |
|                                          |                         | or                                                                        |
|                                          |                         | ~/workdir/mappings/illumina_vs_wuhan_qualimap/                            |
+------------------------------------------+-------------------------+---------------------------------------------------------------------------+
| The number of threads to be used         | -nt                     | 14                                                                        |
+------------------------------------------+-------------------------+---------------------------------------------------------------------------+
| The number of windows                    | -nw                     | 400                                                                       |
+------------------------------------------+-------------------------+---------------------------------------------------------------------------+

The complete commandline is::

  qualimap bamqc -bam ~/workdir/mappings/basecall_tiny_porechopped_vs_wuhan.sorted.bam -nw 5000 -nt 14 -c -outdir ~/workdir/mappings/basecall_tiny_porechopped_vs_wuhan_qualimap/

and for Illumina::

  qualimap bamqc -bam ~/workdir/mappings/illumina_vs_wuhan.sorted.bam -nw 5000 -nt 14 -c -outdir ~/workdir/mappings/illumina_vs_wuhan_qualimap/

You can view the results with firefox::

  firefox ~/workdir/mappings/basecall_tiny_porechopped_vs_wuhan_qualimap/qualimapReport.html
  or
  firefox ~/workdir/mappings/illumina_vs_wuhan_qualimap/qualimapReport.html


References
^^^^^^^^^^

**QualiMap** http://qualimap.bioinfo.cipf.es/doc_html/index.html
