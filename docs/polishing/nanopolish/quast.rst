Assembly evaluation with quast
------------------------------

We are going to evaluate our polished assembly. To call ``quast.py`` we have to provide a reference genome and an assembly as before. We will include our original canu assembly for comparison::
   
  quast.py -t 16 -o ~/workdir/quast_nanopolished_assembly -R ~/Data/Reference/CXERO_10272017.fna ~/workdir/polishedContig.fasta ~/workdir/canu_assembly/largestContig.fasta

QUAST generates HTML reports including a number of interactive graphics. To access these reports, copy the
quast directory to your `www` folder::

  cp -r ~/workdir/quast_nanopolished_assembly ~/www/

You can load the reports in your web browser::

  http://YOUR_OPENSTACK_INSTANCE_IP/quast_nanopolished_assembly/summary/report.html

Compare to the previous results without polishing.


It is possible to further polish this assembly with pilon. We have prepared these datasets for you - copy them into your workdir::

  cp -r ~/Results/Pilon_after_nanopolish/ ~/workdir/
  
We will have a look on the quast results containing all polished and unpolished assemblies::

  quast.py -t 16 -o ~/workdir/quast_polish_compare/ -R ~/Data/Reference/CXERO_10272017.fna ~/workdir/canu_assembly/largestContig.fasta ~/workdir/Pilon/Pilon_round1.fasta ~/workdir/Pilon/Pilon_round2.fasta ~/workdir/Pilon/Pilon_round3.fasta ~/workdir/Pilon/Pilon_round4.fasta ~/workdir/polishedContig.fasta ~/workdir/Pilon_after_nanopolish/Pilon_round1.fasta ~/workdir/Pilon_after_nanopolish/Pilon_round2.fasta ~/workdir/Pilon_after_nanopolish/Pilon_round3.fasta ~/workdir/Pilon_after_nanopolish/Pilon_round4.fasta ~/workdir/Pilon_after_nanopolish/Pilon_round5.fasta
  
  cp -r ~/workdir/quast_polish_compare/ ~/www/

Inspect in your web browser::

  http://YOUR_OPENSTACK_INSTANCE_IP/quast_polish_compare/summary/report.html

References
^^^^^^^^^^

**quast** http://sourceforge.net/projects/quast
