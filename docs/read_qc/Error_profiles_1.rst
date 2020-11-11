
Generating Error Profiles
-------------------------

Mapping the data
^^^^^^^^^^^^^^^^

GraphMap is a novel mapper targeted at aligning long, error-prone third-generation sequencing data.
It is designed to handle Oxford Nanopore MinION 1d and 2d reads with very high sensitivity and accuracy, and also presents a significant improvement over the state-of-the-art for PacBio read mappers.

GraphMap was also designed for ease-of-use: the default parameters can handle a wide range of read lengths and error profiles, including: Illumina, PacBio and Oxford Nanopore. Currently, graphmapper provides two core modules, an aligner and an overlapper. We're using the aligner here for the mapping procedure.

The shortened usage message of the aligner::

  graphmap align --help

Usage:
	
 graphmap [options] -r <reference_file> -d <reads_file> -o <output_sam_path>

 Options
 
  Input/Output options
    -r, --ref                STR   Path to the reference sequence (fastq or fasta).
    -i, --index              STR   Path to the index of the reference sequence. If not specified, index is generated in
                                   the same folder as the reference file, with .gmidx extension. For non-parsimonious
                                   mode, secondary index .gmidxsec is also generated.
    -d, --reads              STR   Path to the reads file.
    -o, --out                STR   Path to the output file that will be generated.
        --gtf                STR   Path to a General Transfer Format file. If specified, a transcriptome will be built
                                   from the reference sequence and used for mapping. Output SAM alignments will be in
                                   genome space (not transcriptome).
    -K, --in-fmt             STR   Format in which to input reads. Options are:
                                    auto  - Determines the format automatically from file extension.
                                    fastq - Loads FASTQ or FASTA files.
                                    fasta - Loads FASTQ or FASTA files.
                                    gfa   - Graphical Fragment Assembly format.
                                    sam   - Sequence Alignment/Mapping format. [auto]
    -L, --out-fmt            STR   Format in which to output results. Options are:
                                    sam  - Standard SAM output (in normal and '-w overlap' modes).
                                    m5   - BLASR M5 format. [sam]

  General-purpose pre-set options
    -x, --preset             STR   Pre-set parameters to increase sensitivity for different sequencing technologies.
                                   Valid options are
				   illumina  - Equivalent to '-a gotoh -w normal -M 5 -X 4 -G 8 -E 6'
				   overlap   - Equivalent to '-a anchor -w normal --overlapper --evalue 1e0                                    --ambiguity 0.50 --secondary'
				   sensitive - Equivalent to '--freq-percentile 1.0 --minimizer-window 1'

  Alignment options
    -a, --alg                STR   Specifies which algorithm should be used for alignment. Options are
                                    sg          - Myers' bit-vector approach. Semiglobal. Edit dist. alignment.
                                    sggotoh     - Gotoh alignment with affine gaps. Semiglobal.
                                    anchor      - anchored alignment with end-to-end extension.
                                                  Uses Myers' global alignment to align between anchors.
                                    anchorgotoh - anchored alignment with Gotoh.
                                                  Uses Gotoh global alignment to align between anchors. [anchor]    
    -M, --match              INT   Match score for the DP alignment. Ignored for Myers alignment. [5]
    -X, --mismatch           INT   Mismatch penalty for the DP alignment. Ignored for Myers alignment. [4]
    -G, --gapopen            INT   Gap open penalty for the DP alignment. Ignored for Myers alignment. [8]
    -E, --gapext             INT   Gap extend penalty for the DP alignment. Ignored for Myers alignment. [6]
    -z, --evalue             FLT   Threshold for E-value. If E-value > FLT, read will be called unmapped. If FLT < 0.0,
                                   thredhold not applied. [1e0]
    -c, --mapq               INT   Threshold for mapping quality. If mapq < INT, read will be called unmapped. [1]
        --extcigar            -    Use the extended CIGAR format for output alignments. [false]
        --no-end2end          -    Disables extending of the alignments to the ends of the read. Works only for
                                   anchored modes. [false]
        --max-error          FLT   If an alignment has error rate (X+I+D) larger than this, it won't be taken into
                                   account. If >= 1.0, this filter is disabled. [1.0]
        --max-indel-error    FLT   If an alignment has indel error rate (I+D) larger than this, it won't be taken into
                                   account. If >= 1.0, this filter is disabled. [1.0]

  Algorithmic options
    -k                       INT   Graph construction kmer size. [6]    
    -A, --minbases           INT   Minimum number of match bases in an anchor. [12]
    -e, --error-rate         FLT   Approximate error rate of the input read sequences. [0.45]
    -g, --max-regions        INT   If the final number of regions exceeds this amount, the read will be called
                                   unmapped. If 0, value will be dynamically determined. If < 0, no limit is set. [0]    
    -C, --circular            -    Reference sequence is a circular genome. [false]
    -F, --ambiguity          FLT   All mapping positions within the given fraction of the top score will be counted for
                                   ambiguity (mapping quality). Value of 0.0 counts only identical mappings. [0.02]
    -Z, --secondary           -    If specified, all (secondary) alignments within (-F FLT) will be output to a file.
                                   Otherwise, only one alignment will be output. [false]
    -P, --double-index        -    If false, only one gapped spaced index will be used in region selection. If true,
                                   two such indexes (with different shapes) will be used (2x memory-hungry but more
                                   powerful for very high error rates). [false]
        --min-bin-perc       FLT   Consider only bins with counts above FLT * max_bin, where max_bin is the count of
                                   the top scoring bin. [0.75]
        --bin-step           FLT   After a chunk of bins with values above FLT * max_bin is processed, check if there
                                   is one extremely dominant region, and stop the search. [0.25]
        --min-read-len       INT   If a read is shorter than this, it will be marked as unmapped. This value can be
                                   lowered if the reads are known to be accurate. [80]
        --minimizer-window   INT   Length of the window to select a minimizer from. If equal to 1, minimizers will be
                                   turned off. [5]
        --freq-percentile    FLT   Filer the (1.0 - value) percent of most frequent seeds in the lookup process. [0.99]
        --fly-index           -    Index will be constructed on the fly, without storing it to disk. If it already
                                   exists on disk, it will be loaded unless --rebuild-index is specified. [false]


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

Forward the output of samtools stats into a file called::

  ~/workdir/mappings/basecall_tiny_porechopped_<number>_vs_wuhan.stats

Inspect the results using less or more. If you are stuck - get help on the next page.


References
^^^^^^^^^^

**Samtools** http://samtools.sourceforge.net/

