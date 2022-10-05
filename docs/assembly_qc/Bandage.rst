Assembly Graph inspection with Bandage
==================

This did'nt work out very well, let's have a look on the assembly graph with Bandage.

Bandage is a program for visualising de novo assembly graphs. By displaying connections which are not present in the contigs file, Bandage opens up new possibilities for analysing *de novo* assemblies.

You can start Bandage with::

  Bandage

and load the following file::

   ~/workdir/assembly/small_assembly/assembly.contigs.gfa

and click on "Draw Graph"
This is the assembly graph of our Nanopore Assembly. You can BLAST your contigs vs each other to identify further assembly problems  by clicking "Create/View BLAST search".

There seem to be problems with overlaps in the assembly and also unequally distributed coverage. canu does not seem to handle our data well. If you want, you can try to get a better assembly. Other parameters you might want to change are mentioned in the canu part, others might be::

  correctedErrorRate (default 0.144)
  utgErrorRate (default 0.144)
  
We will now use a different dataset to get a complete assembly.

References
^^^^^^^^^^

**Bandage** https://rrwick.github.io/Bandage/
  
