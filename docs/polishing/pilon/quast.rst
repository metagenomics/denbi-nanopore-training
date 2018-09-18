Assembly evaluation with quast
------------------------------

As usual, we are going to use quast for assembly evaluation::

  cd
  quast.py -t 14 -o ~/workdir/quast_pilon -R ~/workdir/Data/Reference/CXERO_10272017.fna ~/workdir/canu_assembly/largestContig.fasta ~/workdir/Pilon/Pilon_round1.fasta ~/workdir/Pilon/Pilon_round2.fasta ~/workdir/Pilon/Pilon_round3.fasta ~/workdir/Pilon/Pilon_round4.fasta

QUAST generates HTML reports including a number of interactive graphics. To access these reports, open them using your Cloud9 Interface.



References
^^^^^^^^^^

**quast** http://sourceforge.net/projects/quast
