
Assembly with canu(3)
==================

Run trimming and assembly
--------------------------

The command to run the canu trimming and assembly is::

  canu -trim-assemble
  
with the following parameters:

+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| What?                                        | parameter               | Our value                                                                   |
+==============================================+=========================+=============================================================================+
| The input read file                          | -nanopore-corrected     | ~/workdir/assembly/small_correct/assembly.correctedReads.fasta.gz           |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| The output directory                         | -d                      | ~/workdir/assembly/small_assembly                                           |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| Prefix for output files                      | -p                      | assembly                                                                    |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| Use a grid engine                            | useGrid                 | false                                                                       |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| Genome Size                                  | genomeSize              | 30k                                                                         |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| Minimum Read Length                          | minReadLength           | 300                                                                         |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| Minimum Overlap Length                       | minOverlapLength        | 20 or try out different value                                               |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| *Optional:* Coverage of corrected reads      | corOutCoverage          | something smaller than our coverage (~600)                                  |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| *Optional:* Min coverage for corrected reads | corMinCoverage          | 0 to get all                                                                |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| *Optional:* Correction Sensitivity           | corMhapSensitivity      | normal                                                                      |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+


The corOutCoverage parameter defines to which coverage the reads are corrected, longest reads are corrected first. It is advisable to set this parameter high, to get more sequences into the assembly. corMinCoverage set to low value, will report low covered reads as well and corMhapSensitivity=normal is advised for higher coverage.



The complete command is::

  canu -trim-assemble -d ~/workdir/assembly/small_assembly -p assembly useGrid=false -nanopore-corrected ~/workdir/assembly/small_correct/assembly.correctedReads.fasta.gz genomeSize=30k minReadLength=300 minOverlapLength=20

In the next step, we will evaluate our assembly.


References
^^^^^^^^^^

**Canu** https://github.com/marbl/canu
  
**Minimap2** https://github.com/lh3/minimap2

**BWA** http://bio-bwa.sourceforge.net/

**samtools** http://www.htslib.org  

