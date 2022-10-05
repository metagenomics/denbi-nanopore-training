Length filtering
----------------

First of all, if not active, activate the artic-ncov2019 conda environment::

  conda activate artic-ncov2019


The we will use the command::

  artic guppyplex 

to perform a length filtering on the basecalled data and combine all reads into one single file::

  usage: artic guppyplex [-h] [-q] --directory directory
                         [--max-length max_length] [--min-length min_length]
                         [--quality quality] [--sample sample]
                         [--skip-quality-check] [--prefix PREFIX]
                         [--output output]

  optional arguments:
    -h, --help            show this help message and exit
    -q, --quiet           Do not output warnings to stderr
    --directory directory
                          Basecalled and demultiplexed (guppy) results directory
    --max-length max_length
                          remove reads greater than read length
    --min-length min_length
                          remove reads less than read length
    --quality quality     remove reads against this quality filter
    --sample sample       sampling frequency for random sample of sequence to
                          reduce excess
    --skip-quality-check  Do not filter on quality score (speeds up)
    --prefix PREFIX       Prefix for guppyplex files
    --output output       FASTQ file to write

**Task**: Use ``artic guppyplex`` to filter for reads with a minimum size of 400 and a maximum size of 700. Your output files should be named::

  ~/workdir/data_artic/basecall_filtered_01.fastq

**Do the filtering for the first (01) of the 5 datasets only and stick to that dataset for the next parts. When you are quick, you can repeat the procedure for the remaining datasets later.**

References
^^^^^^^^^^

**ARTIC bioinformatics SOP**  https://artic.network/ncov-2019/ncov2019-bioinformatics-sop.html
