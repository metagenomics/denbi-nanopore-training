Assembly evaluation with quast
------------------------------

We are going to evaluate our polished assembly. To call ``quast.py`` we have to provide a reference genome and an assembly as before. We will include our original canu assembly for comparison::
   
  quast.py -t 16 -o ~/workdir/quast_nanopolished_assembly -R ~/Reference/CXERO_10272017.fna ~/workdir/polishedContig.fasta ~/workdir/canu_assembly/largestContig.fasta

QUAST generates HTML reports including a number of interactive graphics. To access these reports, copy the
quast directory to your `www` folder::

  cp -r ~/workdir/quast_nanopolished_assembly ~/www/

You can load the reports in your web browser::

  http://YOUR_OPENSTACK_INSTANCE_IP/quast_nanopolished_assembly/summary/report.html

Compare to the previous results without polishing.

References
^^^^^^^^^^

**quast** http://sourceforge.net/projects/quast
