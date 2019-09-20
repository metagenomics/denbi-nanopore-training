Assembly evaluation with quast
------------------------------

As usual, we are going to use quast for a final assembly evaluation::

  cd ~/workdir
  quast.py -t 14 -o ~/workdir/quast_final -R ~/workdir/data/Reference.fna ~/workdir/assembly/assembly.contigs.fasta ~/workdir/results/illumina_assembly/contigs.fasta ~/workdir/pilon/pilon_round4.fasta ~/workdir/racon/racon.fasta ~/workdir/racon_medaka/consensus.fasta ~/workdir/racon_medaka_pilon/pilon_round1.fasta ~/workdir/racon_medaka_pilon/pilon_round2.fasta ~/workdir/racon_medaka_pilon/pilon_round3.fasta ~/workdir/racon_medaka_pilon/pilon_round4.fasta ~/workdir/results/nanopolish/polished_genome.fasta

QUAST generates HTML reports including a number of interactive graphics. To access these reports, open a file browser.



References
^^^^^^^^^^

**quast** http://sourceforge.net/projects/quast
