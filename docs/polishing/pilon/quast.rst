Assembly evaluation with quast
------------------------------

As usual, we are going to use quast for assembly evaluation::

  cd
  quast.py -t 16 -o ~/workdir/quast_pilon -R ~/workdir/Data/Reference/CXERO_10272017.fna ~/workdir/canu_assembly/largestContig.fasta ~/workdir/Pilon/Pilon_round1.fasta ~/workdir/Pilon/Pilon_round2.fasta ~/workdir/Pilon/Pilon_round3.fasta ~/workdir/Pilon/Pilon_round4.fasta

QUAST generates HTML reports including a number of interactive graphics. To access these reports, copy the
quast directory to your `www` folder::

  cp -r ~/workdir/quast_pilon ~/www/

You can load the reports in your web browser::

  http://YOUR_OPENSTACK_INSTANCE_IP/quast_pilon/report.html

Compare to the previous results without polishing.

References
^^^^^^^^^^

**quast** http://sourceforge.net/projects/quast
