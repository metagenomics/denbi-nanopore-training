
Generating Error Profiles
-------------------------

Inferring error profiles using samtools
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

After mapping the reads on the reference Genome, we can infer various statistics as e.g., number of succesful aligned reads and bases, or number of mismatches and indels, and so on. For this you could easily use the tool collection **samtools**, which offers a range of simple CLI modules all operating on mapping output (SAM and BAM format). We will use the ``stats`` module now::
 
	Usage: samtools stats [OPTIONS] file.bam
	       samtools stats [OPTIONS] file.bam chr:from-to
	Options:
	    -c, --coverage <int>,<int>,<int>    Coverage distribution min,max,step [1,1000,1]
	    -d, --remove-dups                   Exclude from statistics reads marked as duplicates
	    -X, --customized-index-file         Use a customized index file
	    -f, --required-flag  <str|int>      Required flag, 0 for unset. See also `samtools flags` [0]
	    -F, --filtering-flag <str|int>      Filtering flag, 0 for unset. See also `samtools flags` [0]
		--GC-depth <float>              the size of GC-depth bins (decreasing bin size increases memory requirement) [2e4]
	    -h, --help                          This help message
	    -i, --insert-size <int>             Maximum insert size [8000]
	    -I, --id <string>                   Include only listed read group or sample name
	    -l, --read-length <int>             Include in the statistics only reads with the given read length [-1]
	    -m, --most-inserts <float>          Report only the main part of inserts [0.99]
	    -P, --split-prefix <str>            Path or string prefix for filepaths output by -S (default is input filename)
	    -q, --trim-quality <int>            The BWA trimming parameter [0]
	    -r, --ref-seq <file>                Reference sequence (required for GC-depth and mismatches-per-cycle calculation).
	    -s, --sam                           Ignored (input format is auto-detected).
	    -S, --split <tag>                   Also write statistics to separate files split by tagged field.
	    -t, --target-regions <file>         Do stats in these regions only. Tab-delimited file chr,from,to, 1-based, inclusive.
	    -x, --sparse                        Suppress outputting IS rows where there are no insertions.
	    -p, --remove-overlaps               Remove overlaps of paired-end reads from coverage and base count computations.
	    -g, --cov-threshold <int>           Only bases with coverage above this value will be included in the target percentage computation [0]
	      --input-fmt-option OPT[=VAL]
		       Specify a single input file format option in the form
		       of OPTION or OPTION=VALUE
	      --reference FILE
		       Reference sequence FASTA FILE [null]
	  -@, --threads INT
		       Number of additional threads to use [0]
	      --verbosity INT
		       Set level of verbosity

You can use::
  
  -@ 14

to use more than one thread, but this shouldn't be necessary for the size of our dataset.

Forward the output of samtools stat into a file called::

  ~/workdir/mappings/basecall_tiny_porechopped_<number>_vs_wuhan.stats

Inspect the results using less or more. If you are stuck - get help on the next page.


References
^^^^^^^^^^

**Samtools** http://samtools.sourceforge.net/
