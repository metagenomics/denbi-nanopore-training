
Assembly of WGS dataset
==================

We will now assemble the WGS dataset.

Run assembly
------------

The command to run the canu assembly is::

  canu
  
Without -correct or -trim-assemble, the complete assembly will run in one step.


Use the appropriate parameters, which are::

 -nanopore-raw <fastq file with reads>
 -d <directory where the assembly should be stored>
 -p assembly (this will be the prefix for your assembly files)
  
The output directory should be named::

  ~/workdir/assembly/assembly_wgs/

In addition, we need some further parameters::
  
  useGrid=false (we don't have a cluster)
  minReadLength=<minimum read length>
  minOverlapLength=<minimum overlap length>
  genomeSize=<size of the target genome, i.e. 50k>
  
You already know these parameters from our previous assembly. We don't have amplicon data now, but the reads are still quite short (for Nanopore reads). The default minReadLength is 1000, this might remove quite a bit of our reads, so choose something smaller. minOverlapLength does not need to be as small as for the amplicon dataset, since we have larger overlaps now. It still needs to be smaller than the minReadLength. 

Try parameters, that you think, make sense. If your assembly sucks - try again. Or check the next page for parameters that should work.


References
^^^^^^^^^^

**Canu** https://github.com/marbl/canu

