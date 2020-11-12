
Assembly with canu(2)
==================

Generate corrected reads
------------------------


The command to run the canu correction is::

  canu -correct
  
with the following parameters:

-nanopore-raw <fastq file with reads>
-d <directory where the assembly should be stored>
-p assembly (this will be the prefix for your assembly files)
and in addition, we need some further parameters:

useGrid=false (we don't have a cluster)
minReadLength=<minimum read length>
minOverlapLength=<minimum overlap length>
genomeSize=<size of the target genome, i.e. 50k>

+----------------------------------------------+-------------------------+------------------------------------------------------------------+
| What?                                        | parameter               | Our value                                                        |
+==============================================+=========================+==================================================================+
| The input read file                          | -nanopore-raw           | ~/workdir/data_artic/basecall_small_porechopped_<number>.fastq.gz|
+----------------------------------------------+-------------------------+------------------------------------------------------------------+
| The output directory                         | -d                      | ~/workdir/assembly/small_01_correct                              |
+----------------------------------------------+-------------------------+------------------------------------------------------------------+
| Prefix for output files                      | -p                      | assembly                                                         |
+----------------------------------------------+-------------------------+------------------------------------------------------------------+
| Use a grid engine                            | useGrid                 | fals                                                             |
+----------------------------------------------+-------------------------+------------------------------------------------------------------+
| Genome Size                                  | genomeSize              | 30k                                                              |
+----------------------------------------------+-------------------------+------------------------------------------------------------------+
| Minimum Read Length                          | minReadLength           | 300                                                              |
+----------------------------------------------+-------------------------+------------------------------------------------------------------+
| Minimum Overlap Length                       | minOverlapLength        | 20 or try out different value                                    |
+----------------------------------------------+-------------------------+------------------------------------------------------------------+
| *Optional:* Coverage of corrected reads      | corOutCoverage          | something smaller than our coverage (~600)                       |
+----------------------------------------------+-------------------------+------------------------------------------------------------------+
| *Optional:* Min coverage for corrected reads | corMinCoverage          | 0 to get all                                                     |
+----------------------------------------------+-------------------------+------------------------------------------------------------------+
| *Optional:* Correction Sensitivity           | corMhapSensitivity      | normal                                                           |
+----------------------------------------------+-------------------------+------------------------------------------------------------------+


The corOutCoverage parameter defines to which coverage the reads are corrected, longest reads are corrected first. It is advisable to set this parameter high, to get more sequences into the assembly. corMinCoverage set to low value, will report low covered reads as well and corMhapSensitivity=normal is advised for higher coverage.



The complete command is::

  canu -correct -d ~/workdir/data_artic/small_<number>_correct -p assembly useGrid=false -nanopore-raw ~/workdir/data_artic/basecall_small_porechopped_01.fastq.gz genomeSize=30k minReadLength=300 minOverlapLength=20



Get error statistics
--------------------

TODO: Wollen wir das noch machen? 

Generate and assemble trimmed reads
-----------------------------------

The trimming stage identifies unsupported regions in the input and trims or splits reads to their longest supported range. The assembly stage makes a final pass to identify sequencing errors; constructs the best overlap graph (BOG); and outputs contigs, an assembly graph, and summary statistics::

  canu -trim-assemble -d ~/workdir/assembly_small -p assembly genomeSize=2M useGrid=false -nanopore-corrected ~/workdir/correct_small/assembly.correctedReads.fasta.gz

After that is done, inspect the results. We can get a quick view on the number of generated contigs with::

  grep '>' ~/workdir/assembly_small/assembly.contigs.fasta

**If there is time**, we start the actual assembly with all data now::

  Group 1:
  canu -d ~/workdir/assembly -p assembly "genomeSize=4.3M" useGrid=false -nanopore-raw ~/workdir/basecall/basecall_trimmed.fastq.gz
  Group 2:
  canu -d ~/workdir/assembly -p assembly "genomeSize=6.8M" useGrid=false -nanopore-raw ~/workdir/basecall/basecall_trimmed.fastq.gz

**Otherwise**, copy the precomputed assembly with the complete dataset into your working directory::

  cp -r ~/workdir/results/assembly/ ~/workdir/

and have a quick look on the number of contigs::

  grep '>' ~/workdir/assembly/assembly.contigs.fasta




References
^^^^^^^^^^

**Canu** https://github.com/marbl/canu
  
