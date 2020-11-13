WGS Assembly evaluation with QUAST(2)
==============================

We run::

  quast.py
  
with the following parameters:

+------------------------------------------+-------------------------+--------------------------------------------------------------------+
| What?                                    | parameter               | Our value                                                          |
+==========================================+=========================+====================================================================+
| The input assembly                       | positional              | ~/workdir/assembly/assembly_wgs/assembly.contigs.fasta/            |
+------------------------------------------+-------------------------+--------------------------------------------------------------------+ 
| The output directory                     | -o                      | ~/workdir/assembly/assembly_wgs/quast/                             |
+------------------------------------------+-------------------------+--------------------------------------------------------------------+
| The number of threads to be used         | -t                      | 14                                                                 |
+------------------------------------------+-------------------------+--------------------------------------------------------------------+


The complete commandline is::

  quast.py -t 14 -o ~/workdir/assembly/assembly_wgs/quast/ -r ~/workdir/wuhan.fasta ~/workdir/assembly/assembly_wgs/assembly.contigs.fasta/  

QUAST generates HTML reports including a number of interactive graphics. To access these reports, open them using a file browser::

  firefox ~/workdir/assembly/assembly_wgs/quast/report.html
  
Inspect the results - are you satisfied with the assembly now?

References
^^^^^^^^^^

**quast** http://sourceforge.net/projects/quast
