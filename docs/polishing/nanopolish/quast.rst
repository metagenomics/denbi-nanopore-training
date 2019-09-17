Assembly evaluation with quast
------------------------------

We are going to evaluate our polished assembly. To call ``quast.py`` we have to provide a reference genome and an assembly as before. We will include our original canu assembly for comparison::
   
  quast.py -t 14 -o ~/workdir/quast_nanopolished_assembly -R ~/workdir/Data/Reference/CXERO_10272017.fna ~/workdir/polishedContig.fasta ~/workdir/canu_assembly/largestContig.fasta

QUAST generates HTML reports including a number of interactive graphics. Access the reports using the Cloud9 interface.::

It is possible to further polish this assembly with pilon. We have prepared these datasets for you - copy them into your workdir::

  cp -r ~/workdir/Results/Pilon_after_nanopolish/ ~/workdir/
  
We will have a look on the quast results containing all polished and unpolished assemblies::

  quast.py -o quast -r data/Reference.fna -t 14 assembly/assembly.contigs.fasta nanopolish/polished_genome.fasta pilon/pilon_round4.fasta medaka/consensus.fasta

  quast.py -t 14 -o ~/workdir/quast_polish_compare/ -R ~/workdir/Data/Reference/CXERO_10272017.fna ~/workdir/canu_assembly/largestContig.fasta ~/workdir/Pilon/Pilon_round1.fasta ~/workdir/Pilon/Pilon_round2.fasta ~/workdir/Pilon/Pilon_round3.fasta ~/workdir/Pilon/Pilon_round4.fasta ~/workdir/polishedContig.fasta ~/workdir/Pilon_after_nanopolish/Pilon_round1.fasta ~/workdir/Pilon_after_nanopolish/Pilon_round2.fasta ~/workdir/Pilon_after_nanopolish/Pilon_round3.fasta ~/workdir/Pilon_after_nanopolish/Pilon_round4.fasta ~/workdir/Pilon_after_nanopolish/Pilon_round5.fasta
  
QUAST generates HTML reports including a number of interactive graphics. To access these reports, open them using your Cloud9 Interface.


References
^^^^^^^^^^

**quast** http://sourceforge.net/projects/quast
