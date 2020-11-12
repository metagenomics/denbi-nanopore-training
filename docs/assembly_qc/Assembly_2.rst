
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

Let's get some error statistics for the corrected reads. Map the corrected reads to the Wuhan reference::

  minimap2 -a ~/workdir/wuhan.fasta ~/workdir/assembly/small_<number>_correct/assembly.correctedReads.fasta.gz | samtools view -b - | samtools sort - > ~/workdir/assembly/small_<number>_correct/corrected_reads_vs_wuhan.sorted.bam
  
Then run qualimap::

  qualimap bamqc -bam ~/workdir/assembly/small_01_correct/corrected_reads_vs_wuhan.sorted.bam -nw 5000 -nt 14 -c -outdir ~/workdir/assembly/small_<number>_correct/qualimap/
  
Then open the results in a web browser::

  firefox ~/workdir/assembly/small_01_correct/qualimap/qualimapReport.html

Inspect the results, how much did our error rate decrease?

Generate and assemble trimmed reads
-----------------------------------

The trimming stage identifies unsupported regions in the input and trims or splits reads to their longest supported range. The assembly stage makes a final pass to identify sequencing errors; constructs the best overlap graph (BOG); and outputs contigs, an assembly graph, and summary statistics.

Now run the trimming and assembly step using the following command::

  canu -trim-assemble
  
You need to define the following parameters::

  -nanopore-corrected <corrected reads file>
  
The output directory should be named::

  ~/workdir/assembly/small_<number>_assembly/

In addition, we need some further parameters::
  
  useGrid=false (we don't have a cluster)
  minReadLength=<minimum read length>
  minOverlapLength=<minimum overlap length>
  genomeSize=<size of the target genome, i.e. 50k>
  
Use the same parameters as before (although you could use different settings here).

Go to the next page, if you need help.


References
^^^^^^^^^^

**Canu** https://github.com/marbl/canu
  
**Minimap2** https://github.com/lh3/minimap2

**BWA** http://bio-bwa.sourceforge.net/

**samtools** http://www.htslib.org  

