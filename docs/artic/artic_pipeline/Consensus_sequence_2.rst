Generating consensus sequence (2)
----------------

First of all, if not active, activate the artic-ncov2019 conda environment::

  conda activate artic-ncov2019
  
Then use the command::

  artic minion 

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


Enter the newly created results directory first::

  cd ~/workdir/results_artic/

Then you can run the ARTIC pipeline for one dataset::

  artic minion --medaka --normalise 200 --threads 14 --scheme-directory ~/artic-ncov2019/primer_schemes --read-file ~/workdir/data_artic/basecall_filtered_<number>.fastq nCoV-2019/V3 barcode_<number>

**Perform that step for one dataset only to save time. Do the other datasets later, when there is time left.**

A loop to process all datasets would look like this::

  for i in {1..5} 
  do
  artic minion --medaka --normalise 200 --threads 14 --scheme-directory ~/artic-ncov2019/primer_schemes --read-file ~/workdir/data_artic/basecall_filtered_0$i.fastq nCoV-2019/V3 barcode_0$i
  done
  
When you are done, consensus files have been generated::

  ~/workdir/results_artic/barcode_<number>.consensus.fasta
  
If you want, you can map the consensus to the Wuhan reference and view the results in GenomeView, or use QUAST, to compare the sequences.
  

References
^^^^^^^^^^

**ARTIC bioinformatics SOP**  https://artic.network/ncov-2019/ncov2019-bioinformatics-sop.html
