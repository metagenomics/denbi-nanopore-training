Generating consensus sequence (2)
----------------

First of all, if not active, activate the artic-ncov2019 conda environment::

  conda activate artic-ncov2019
  
Then use the command::

  artic guppyplex 

with the following parameters:

+------------------------------------------+-------------------------+--------------------------------------------------------------------+
| What?                                    | parameter               | Our value                                                          |
+==========================================+=========================+====================================================================+
| Use medaka                               | --medaka                                                                                     |
+------------------------------------------+-------------------------+--------------------------------------------------------------------+ 
| The directory containing primer schemes  | --scheme-directory      | ~/artic-ncov2019/primer_schemes                                    |
+------------------------------------------+-------------------------+--------------------------------------------------------------------+ 
| The input read file                      | --read-file             | ~/workdir/data_artic/basecall_filtered_<number>.fastq              |
+------------------------------------------+-------------------------+--------------------------------------------------------------------+
| Number of threads to use                 | --threads               | 14                                                                 |
+------------------------------------------+-------------------------+--------------------------------------------------------------------+
| Normalise to max 200fold coverage        | --normalise             | 200                                                                |
+------------------------------------------+-------------------------+--------------------------------------------------------------------+
| The primer scheme to use                 | positional (1)          | nCoV-2019/V3                                                       |
+------------------------------------------+-------------------------+--------------------------------------------------------------------+
| The sample name (prefix for output)      | positional (2)          | barcode_<nubmer>                                                   |
+------------------------------------------+-------------------------+--------------------------------------------------------------------+

Commands::

  artic minion --medaka --normalise 200 --threads 4 --scheme-directory ~/artic-ncov2019/primer_schemes --read-file ~/workdir/data_artic/basecall_small_filtered_01.fastq nCoV-2019/V3 samplename

  for i in {1..5} 
  do
  artic minion --medaka --normalise 200 --threads 14 --scheme-directory ~/artic-ncov2019/primer_schemes --read-file ~/workdir/data_artic/basecall_small_filtered_0$i.fastq nCoV-2019/V3 barcode_0$i
  done
  

References
^^^^^^^^^^

**ARTIC bioinformatics SOP**  https://artic.network/ncov-2019/ncov2019-bioinformatics-sop.html
