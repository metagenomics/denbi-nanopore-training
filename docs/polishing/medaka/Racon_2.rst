Polishing with Racon(2)
------

We call racon::

  racon
  
with the following parameters::

~/workdir/data_wgs/Cov2_HK_WGS_small_porechopped.fastq.gz
~/workdir/mappings/Cov2_HK_WGS_small_porechopped_vs_wuhan.sam
~/workdir/wuhan.fasta

+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| What?                                        | parameter               | Our value                                                                   |
+==============================================+=========================+=============================================================================+
| The input read file                          | positional (1)          | ~/workdir/data_wgs/Cov2_HK_WGS_small_porechopped.fastq.gz                   |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| The mapping file                             | positional (2)          | ~/workdir/mappings/Cov2_HK_WGS_small_porechopped_vs_assembly_wgs.sam        |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| The reference file                           | positional (3)          | ~/workdir/assemmbly/assembly_wgs/assemby.contigs.fasta                      |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| Number of threads                            | -t                      | 14                                                                          |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| Racon parameters for medaka                  |-m 8 -x -6 -g -8 -w 500                                                                                |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| Output file                                  | forward with ">"        | ~/workdir/assembly/assembly_wgs/racon.fasta                                 |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+


Then we can call racon with our mapping, the read file and the assembly file. We use 14 threads to do this::

  racon -m 8 -x -6 -g -8 -w 500 -t 14 ~/workdir/data_wgs/Cov2_HK_WGS_small_porechopped.fastq.gz  ~/workdir/mappings/Cov2_HK_WGS_small_porechopped_vs_assembly_wgs.sam ~/workdir/assembly/assembly_wgs/assembly.contigs.fasta > ~/workdir/assembly/assembly_wgs/racon.fasta

References
^^^^^^^^^^

**racon** https://github.com/isovic/racon
