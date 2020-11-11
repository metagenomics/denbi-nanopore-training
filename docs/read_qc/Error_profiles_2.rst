
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

  Other options
    -t, --threads            INT   Number of threads to use. If '-1', number of threads will be equal to min(24, num_cores/2). [-1]
    -v, --verbose            INT   Verbose level. If equal to 0 nothing except strict output will be placed on stdout. [5]    
    -h, --help                -    View this help. [false]
 

We now use graphmap to align the different read sets to the reference, starting with the nanopore reads::

  cd ~/workdir
  mkdir ~/workdir/map_to_ref
  graphmap align -r ~/workdir/data/Reference.fna -t 14 -C -d ~/workdir/basecall/basecall.fastq.gz -o ~/workdir/map_to_ref/nanopore.graphmap.sam >  ~/workdir/map_to_ref/nanopore.graphmap.sam.log 2>&1 
  
For the illumina reads we will use another aligner, as this one is more suited for this kind of data. But before we can do so, we need to create an index structure on the reference::
  
  bwa index ~/workdir/data/Reference.fna
  bwa mem -t 14 ~/workdir/data/Reference.fna ~/workdir/data/illumina/Illumina_R1.fastq.gz ~/workdir/data/illumina/Illumina_R2.fastq.gz > ~/workdir/map_to_ref/illumina.bwa.sam
  
Inferring error profiles using samtools
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

After mapping the reads on the reference Genome, we can infer various statistics as e.g., number of succesful aligned reads and bases, or number of mismatches and indels, and so on. For this you could easily use the tool collection **samtools**, which offers a range of simple CLI modules all operating on mapping output (SAM and BAM format). We will use the ``stats`` module now::
 
  samtools stats -d -@ 14 ~/workdir/map_to_ref/nanopore.graphmap.sam > ~/workdir/map_to_ref/nanopore.graphmap.sam.stats
  samtools stats -d -@ 14 ~/workdir/map_to_ref/illumina.bwa.sam > ~/workdir/map_to_ref/illumina.bwa.sam.stats

We can inspect these results now by simply view at the top 40 lines of the output::
  
  head -n 40 ~/workdir/map_to_ref/nanopore.graphmap.sam.stats
  head -n 40 ~/workdir/map_to_ref/illumina.bwa.sam.stats

Enhanced mapping statistics
^^^^^^^^^^^^^^^^^^^^^^^^^^^

To get a more in depth info on the actual accuracy of the data at hand, including the genome coverage, we're going to use a more comprehensive and interactive software comparable to FastQC which is called **Qualimap**.

First, we convert the SAM files into BAM format and sort them::

  cd ~/workdir
  samtools view -@ 4 -bS  ~/workdir/map_to_ref/nanopore.graphmap.sam | samtools sort - -@ 8 -o ~/workdir/map_to_ref/nanopore.graphmap.sorted.bam
  samtools view -@ 4 -bS ~/workdir/map_to_ref/illumina.bwa.sam | samtools sort - -@ 8 -o ~/workdir/map_to_ref/illumina.bwa.sorted.bam

Then we can run **qualimap** on those BAM files now::
  
  qualimap bamqc -bam ~/workdir/map_to_ref/nanopore.graphmap.sorted.bam -nw 5000 -nt 14 -c -outdir ~/workdir/map_to_ref/nanopore.graphmap
  qualimap bamqc -bam ~/workdir/map_to_ref/illumina.bwa.sorted.bam -nw 5000 -nt 14 -c -outdir ~/workdir/map_to_ref/illumina.graphmap

Qualimap can also be run interactively.

References
^^^^^^^^^^

**GraphMap** https://github.com/isovic/graphmap

**BWA** http://bio-bwa.sourceforge.net/

**Samtools** http://samtools.sourceforge.net/

**QualiMap** http://qualimap.bioinfo.cipf.es/doc_html/index.html
