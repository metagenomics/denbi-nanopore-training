Assembly evaluation with quast
------------------------------

As usual, we are going to use quast for assembly evaluation::

  cd ~/workdir
  quast.py -t 14 -o ~/workdir/quast_medaka -R ~/workdir/data/Reference.fna ~/workdir/assembly/assembly.contigs.fasta ~/workdir/racon/racon.fasta  ~/workdir/medaka/consensus.fasta ~/workdir/racon_medaka/consensus.fasta

QUAST generates HTML reports including a number of interactive graphics. To access these reports, open a file browser.



References
^^^^^^^^^^

**quast** http://sourceforge.net/projects/quast
