Generating consensus sequence
----------------

First of all, if not active, activate the artic-ncov2019 conda environment::

  conda activate artic-ncov2019
  
We will now use the artic pipeline to call variants in the Wuhan reference using our amlicon dataset. The command to do that is::

  artic minion
  
There are two tools, that the pipeline uses to call variants:
1) Nanopolish
2) Medaka

Since Medaka is faster and in our experience more accurate we will use the second option. You need to set the appropriate command line flag to do that.

Check the usage for artic minion::

  usage: artic minion [-h] [-q] [--medaka] [--minimap2] [--bwa]
                      [--normalise NORMALISE] [--threads THREADS]
                      [--scheme-directory scheme_directory]
                      [--max-haplotypes max_haplotypes] [--read-file read_file]
                      [--fast5-directory FAST5_DIRECTORY]
                      [--sequencing-summary SEQUENCING_SUMMARY]
                      [--skip-nanopolish] [--no-indels] [--dry-run]
                      scheme sample

  positional arguments:
    scheme                The name of the scheme.
    sample                The name of the sample.

  optional arguments:
    -h, --help            show this help message and exit
    -q, --quiet           Do not output warnings to stderr
    --medaka              Use medaka instead of nanopolish for variants
    --minimap2            Use minimap2 (default)
    --bwa                 Use bwa instead of minimap2
    --normalise NORMALISE
                          Normalise down to moderate coverage to save runtime.
    --threads THREADS     Number of threads
    --scheme-directory scheme_directory
                          Default scheme directory
    --max-haplotypes max_haplotypes
                          max-haplotypes value for nanopolish
    --read-file read_file
                          Use alternative FASTA/FASTQ file to <sample>.fasta
    --fast5-directory FAST5_DIRECTORY
                          FAST5 Directory
    --sequencing-summary SEQUENCING_SUMMARY
                          Path to Guppy sequencing summary
    --skip-nanopolish
    --no-indels
    --dry-run

Create a directory for the results and cd into it:: 

  mkdir ~/workdir/results_artic/
  cd ~/workdir/results_artic/

Then run the ``artic minion`` command using medaka, use 14 threads, you can normalise to 200fold coverage to save runtime if you want. You need to set the correct scheme directory (containing primer sequences), which is::

  ~/artic-ncov2019/primer_schemes
  
And as positional arguments, you need to provide::

  (1) the exact primer sequences:
  nCov2019/V3
  
  (2) The samplename:
  barcode_01
  

**Perform that step for the first (01) dataset only to save time. Do the other datasets later, when there is time left.**  

If you are stuck, get help on the next page.


References
^^^^^^^^^^

**ARTIC bioinformatics SOP**  https://artic.network/ncov-2019/ncov2019-bioinformatics-sop.html
