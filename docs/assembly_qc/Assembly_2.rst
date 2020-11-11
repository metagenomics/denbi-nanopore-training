
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

+------------------------------------------+-------------------------+------------------------------------------------------------------+
| What?                                    | parameter               | Our value                                                        |
+==========================================+=========================+==================================================================+
| The input read file                      | -nanopore-raw           | ~/workdir/data_artic/basecall_small_porechopped_<number>.fastq.gz|
+------------------------------------------+-------------------------+------------------------------------------------------------------+
| The output directory                     | -d                      | ~/workdir/assembly/small_01                                      |
+------------------------------------------+-------------------------+------------------------------------------------------------------+
| Prefix for output files                  | -p                      | assembly                                                         |
+------------------------------------------+-------------------------+------------------------------------------------------------------+
| Use a grid engine                        | useGrid                 | fals                                                             |
+------------------------------------------+-------------------------+------------------------------------------------------------------+
| Genome Size                              | genomeSize              | 30k                                                              |
+------------------------------------------+-------------------------+------------------------------------------------------------------+
| Minimum Read Length                      | minReadLength           | 300                                                              |
+------------------------------------------+-------------------------+------------------------------------------------------------------+
| Minimum Overlap Length                   | minOverlapLength        | 20 or try out different value                                    |
+------------------------------------------+-------------------------+------------------------------------------------------------------+
| *Optional:* Coverage of corrected reads  | corOutCoverage          | something smaller than our coverage (~600)                       |
+------------------------------------------+-------------------------+------------------------------------------------------------------+

The corOutCoverage parameter defines to which coverage the reads are corrected, longest reads are corrected first. It is advisable to set this parameter high, to get more sequences into the assembly.


corMinCoverage

The complete command is::

  canu -correct -d ~/workdir/correct_small -p assembly genomeSize=3m useGrid=false -nanopore-raw ~/workdir/basecall_small/basecall.fastq.gz

It is also possible to run multiple correction rounds to eliminate errors. This has been done on a S. cerevisae dataset in the canu publication. We will not do this in this course due to time limitations, but a script to do this, would look like this::

  COUNT=0
   NAME=input.fasta
   for i in `seq 1 10`; do
   canu -correct -p asm -d round$i \
   corOutCoverage=500 corMinCoverage=0 corMhapSensitivity=high \
   genomeSize=12.1m -nanopore-raw $NAME
   NAME="round$i/asm.correctedReads.fasta.gz"
   COUNT=`expr $COUNT + 1`
   done
   canu -p asm -d asm genomeSize=12.1m -nanopore-corrected $NAME utgGraphDeviation=50
  batOptions=”-ca 500 -cp 50”
  done


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
  
