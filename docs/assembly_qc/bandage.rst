Assembly Graph inspection with Bandage
==================

This did'nt work out very well, let's have a look on the assembly graph with Bandage.

Bandage is a program for visualising de novo assembly graphs. By displaying connections which are not present in the contigs file, Bandage opens up new possibilities for analysing *de novo* assemblies.

You can start Bandage with::

  Bandage

and load the following file::

   ~/workdir/assembly/small_<number>_assembly/assembly.contigs.gfa

and click on "Draw Graph"
This is the assembly graph of our Nanopore Assembly. You can BLAST your contigs vs each other to identify further assembly problems  by clicking "Create/View BLAST search".



References
^^^^^^^^^^

**Bandage** https://rrwick.github.io/Bandage/
  
