Polishing with medaka(2)
-----------------------

We call::

  medaka_consensus
  
with the following parameters:

+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| What?                                        | parameter               | Our value                                                                   |
+==============================================+=========================+=============================================================================+
| The input read file                          | -i                      | ~/workdir/data_wgs/Cov2_HK_WGS_small_porechopped.fastq.gz                   |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| The racon polished assembly                  | -d                      | ~/workdir/assembly/assembly_wgs/racon.fasta                                 |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| The output directory                         | -o                      | ~/workdir/assembly/assembly_wgs/medaka                                      |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| Number of threads                            | -t                      | 14                                                                          |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+
| The model                                    | -m                      | r941_min_high_g360                                                          |
+----------------------------------------------+-------------------------+-----------------------------------------------------------------------------+

The complete commandline is::

  medaka_consensus -t 14 -m r941_min_high_g360 -i ~/workdir/data_wgs/Cov2_HK_WGS_small_porechopped.fastq.gz  -d ~/workdir/assembly/assembly_wgs/racon.fasta -o assembly/assembly_wgs/medaka
    
In a next step, we will use quast to compare our assemblies.

If medaka_consensus fails with an error massage try the following an re-run the command above::

  pip install tensorflow

References
^^^^^^^^^^

**medaka** https://github.com/nanoporetech/medaka
