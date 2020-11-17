Length filtering
----------------

First of all, if not active, activate the artic-ncov2019 conda environment::

  conda activate artic-ncov2019
  
Then use the command::

  artic guppyplex 

with the following parameters:

+------------------------------------------+-------------------------+--------------------------------------------------------------------+
| What?                                    | parameter               | Our value                                                          |
+==========================================+=========================+====================================================================+
| The input directory containing the reads | --directory             | ~/workdir/data_artic/basecall_small_<number>/                      |
+------------------------------------------+-------------------------+--------------------------------------------------------------------+ 
| The output file                          | --output                | ~/workdir/data_artic/basecall_small_filtered.fastq                 |
+------------------------------------------+-------------------------+--------------------------------------------------------------------+
| Minimum read length                      | --min-length            | 400                                                                |
+------------------------------------------+-------------------------+--------------------------------------------------------------------+
| Maximum read length                      | --max-length            | 700                                                                |
+------------------------------------------+-------------------------+--------------------------------------------------------------------+
| *(optional)* Skip quality check          | --skip-quality-check                                                                         |
+------------------------------------------+-------------------------+--------------------------------------------------------------------+

Since the quality check has been done along with the basecalling, we can use the flag ``--skip-quality-check``. That will improve runtime, but does not really change much.

To perform the filtering for one dataset, we can use the following command::

  artic guppyplex --skip-quality-check --min-length 400 --max-length 700 --directory ~/workdir/data_artic/basecall_small_<number>/ --output ~/workdir/data_artic/basecall_small_filtered_<number>.fastq
  
Repeat that for all datasets. You could also do that in a loop::

  for i in {1..5}
  do artic guppyplex --skip-quality-check --min-length 400 --max-length 700 --directory ~/workdir/data_artic/basecall_small_0$i --output ~/workdir/data_artic/basecall_small_filtered_0$i.fastq
  done
  
In the next step, we use the filtered reads to generate consensus sequences.

References
^^^^^^^^^^

**ARTIC bioinformatics SOP**  https://artic.network/ncov-2019/ncov2019-bioinformatics-sop.html
