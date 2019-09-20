Assembly evaluation with quast
------------------------------

As usual, we are going to use quast for assembly evaluation::

  cd ~/workdir
  quast.py -t 14 -o ~/workdir/quast_pilon -R ~/workdir/data/Reference.fna ~/workdir/assembly/assembly.contigs.fasta ~/workdir/pilon/pilon_round1.fasta ~/workdir/pilon/pilon_round2.fasta ~/workdir/pilon/pilon_round3.fasta ~/workdir/pilon/pilon_round4.fasta

QUAST generates HTML reports including a number of interactive graphics. To access these reports, open a file browser.



References
^^^^^^^^^^

**quast** http://sourceforge.net/projects/quast
