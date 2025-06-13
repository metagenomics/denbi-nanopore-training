Assembly evaluation with quast
------------------------------

As usual, we are going to use quast for assembly evaluation::

  quast.py -t 14 -o ~/workdir/assembly/assembly_wgs/quast_2/ -r ~/workdir/wuhan.fasta ~/workdir/assembly/assembly_wgs/assembly.contigs.fasta  ~/workdir/assembly/assembly_wgs/racon.fasta  ~/workdir/assembly/assembly_wgs/medaka/consensus.fasta 
  
QUAST generates HTML reports including a number of interactive graphics. To access these reports, open a file browser::

  firefox ~/workdir/assembly/assembly_wgs/quast_2/report.html


References
^^^^^^^^^^

**quast** http://sourceforge.net/projects/quast
