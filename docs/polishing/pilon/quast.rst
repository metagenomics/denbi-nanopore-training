Assembly evaluation with quast
------------------------------

As usual, we are going to use quast for assembly evaluation::

  cd
  quast.py -t 16 -o ~/quast_pilon -R ~/Reference/CXERO_10272017.fna ~/workdir/Pilon/Pilon_round1.fasta
  #and so on for other pilon rounds

QUAST generates HTML reports including a number of interactive graphics. To access these reports, copy the
quast directory to your `www` folder::

  cp -r ~/workdir/quast_pilon ~/www/

You can load the reports in your web browser::

  http://YOUR_OPENSTACK_INSTANCE_IP/pilon_round1/summary/report.html

Compare to the previous results without polishing.
