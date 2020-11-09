Generating Error Profiles
-------------------------

We will now map the Nanopore reads to the Wuhan reference and generate error profiles from the mappings. Due to mutations, this is, of course not 100% accurate, but gives an impression.

Get the reference
^^^^^^^^^^^^^^^^^

First, download the reference from the object store::

  cd ~/workdir/
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/wuhan.fasta.gz
  
And unzip::

  gunzip wuhan.fasta.gz

 
Mapping the data
^^^^^^^^^^^^^^^^

We will use Minimap2 to map the reads to the reference. 

Minimap2 is a versatile sequence alignment program that aligns DNA or mRNA sequences against a large reference database. Typical use cases include: (1) mapping PacBio or Oxford Nanopore genomic reads to the human genome; (2) finding overlaps between long reads with error rate up to ~15%; (3) splice-aware alignment of PacBio Iso-Seq or Nanopore cDNA or Direct RNA reads against a reference genome; (4) aligning Illumina single- or paired-end reads; (5) assembly-to-assembly alignment; (6) full-genome alignment between two closely related species with divergence below ~15%.

Get help for minimap2::

  minimap2 --help
  
  Usage: minimap2 [options] <target.fa>|<target.idx> [query.fa] [...]
  Options:
    Indexing:
      -H           use homopolymer-compressed k-mer (preferrable for PacBio)
      -k INT       k-mer size (no larger than 28) [15]
      -w INT       minimizer window size [10]
      -I NUM       split index for every ~NUM input bases [4G]
      -d FILE      dump index to FILE []
    Mapping:
      -f FLOAT     filter out top FLOAT fraction of repetitive minimizers [0.0002]
      -g NUM       stop chain enlongation if there are no minimizers in INT-bp [5000]
      -G NUM       max intron length (effective with -xsplice; changing -r) [200k]
      -F NUM       max fragment length (effective with -xsr or in the fragment mode) [800]
      -r NUM       bandwidth used in chaining and DP-based alignment [500]
      -n INT       minimal number of minimizers on a chain [3]
      -m INT       minimal chaining score (matching bases minus log gap penalty) [40]
      -X           skip self and dual mappings (for the all-vs-all mode)
      -p FLOAT     min secondary-to-primary score ratio [0.8]
      -N INT       retain at most INT secondary alignments [5]
    Alignment:
      -A INT       matching score [2]
      -B INT       mismatch penalty [4]
      -O INT[,INT] gap open penalty [4,24]
      -E INT[,INT] gap extension penalty; a k-long gap costs min{O1+k*E1,O2+k*E2} [2,1]
      -z INT[,INT] Z-drop score and inversion Z-drop score [400,200]
      -s INT       minimal peak DP alignment score [80]
      -u CHAR      how to find GT-AG. f:transcript strand, b:both strands, n:don't match GT-AG [n]
    Input/Output:
      -a           output in the SAM format (PAF by default)
      -o FILE      output alignments to FILE [stdout]
      -L           write CIGAR with >65535 ops at the CG tag
      -R STR       SAM read group line in a format like '@RG\tID:foo\tSM:bar' []
      -c           output CIGAR in PAF
      --cs[=STR]   output the cs tag; STR is 'short' (if absent) or 'long' [none]
      --MD         output the MD tag
      --eqx        write =/X CIGAR operators
      -Y           use soft clipping for supplementary alignments
      -t INT       number of threads [3]
      -K NUM       minibatch size for mapping [500M]
      --version    show version number
    Preset:
      -x STR       preset (always applied before other options; see minimap2.1 for details) []
                   - map-pb/map-ont: PacBio/Nanopore vs reference mapping
                   - ava-pb/ava-ont: PacBio/Nanopore read overlap
                   - asm5/asm10/asm20: asm-to-ref mapping, for ~0.1/1/5% sequence divergence
                   - splice: long-read spliced alignment
                   - sr: genomic short-read mapping

Create a directory for the mapping results in::

  ~/workdir/mappings/
  
The result of your mappings should be named::

  ~/workdir/mappings/basecall_tiny_<number>_vs_wuhan.sam
  
Use the following options::

  -x <appropriate preset>
  -t <number of threads>

When you are done (or stuck) try to map the Illumina data before you proceed to the next page.

For the Illumina data, we use bwa,  as this one is more suited for this kind of data. But before we can do so, we need to create an index structure on the reference::

  Usage:   bwa index [options] <in.fasta>

  Options: -a STR    BWT construction algorithm: bwtsw, is or rb2 [auto]
           -p STR    prefix of the index [same as fasta name]
           -b INT    block size for the bwtsw algorithm (effective with -a bwtsw) [10000000]
           -6        index files named as <in.fasta>.64.* instead of <in.fasta>.* 

  Warning: `-a bwtsw' does not work for short genomes, while `-a is' and
           `-a div' do not work not for long genomes.

Then do the mapping::

  Usage: bwa mem [options] <idxbase> <in1.fq> [in2.fq]

  Algorithm options:

         -t INT        number of threads [1]
         -k INT        minimum seed length [19]
         -w INT        band width for banded alignment [100]
         -d INT        off-diagonal X-dropoff [100]
         -r FLOAT      look for internal seeds inside a seed longer than {-k} * FLOAT [1.5]
         -y INT        seed occurrence for the 3rd round seeding [20]
         -c INT        skip seeds with more than INT occurrences [500]
         -D FLOAT      drop chains shorter than FLOAT fraction of the longest overlapping chain [0.50]
         -W INT        discard a chain if seeded bases shorter than INT [0]
         -m INT        perform at most INT rounds of mate rescues for each read [50]
         -S            skip mate rescue
         -P            skip pairing; mate rescue performed unless -S also in use

  Scoring options:

         -A INT        score for a sequence match, which scales options -TdBOELU unless overridden [1]
         -B INT        penalty for a mismatch [4]
         -O INT[,INT]  gap open penalties for deletions and insertions [6,6]
         -E INT[,INT]  gap extension penalty; a gap of size k cost '{-O} + {-E}*k' [1,1]
         -L INT[,INT]  penalty for 5'- and 3'-end clipping [5,5]
         -U INT        penalty for an unpaired read pair [17]

         -x STR        read type. Setting -x changes multiple parameters unless overridden [null]
                       pacbio: -k17 -W40 -r10 -A1 -B1 -O1 -E1 -L0  (PacBio reads to ref)
                       ont2d: -k14 -W20 -r10 -A1 -B1 -O1 -E1 -L0  (Oxford Nanopore 2D-reads to ref)
                       intractg: -B9 -O16 -L5  (intra-species contigs to ref)

  Input/output options:

         -p            smart pairing (ignoring in2.fq)
         -R STR        read group header line such as '@RG\tID:foo\tSM:bar' [null]
         -H STR/FILE   insert STR to header if it starts with @; or insert lines in FILE [null]
         -o FILE       sam file to output results to [stdout]
         -j            treat ALT contigs as part of the primary assembly (i.e. ignore <idxbase>.alt file)
         -5            for split alignment, take the alignment with the smallest coordinate as primary
         -q            don't modify mapQ of supplementary alignments
         -K INT        process INT input bases in each batch regardless of nThreads (for reproducibility) []

         -v INT        verbosity level: 1=error, 2=warning, 3=message, 4+=debugging [3]
         -T INT        minimum score to output [30]
         -h INT[,INT]  if there are <INT hits with score >80% of the max score, output all in XA [5,200]
         -a            output all alignments for SE or unpaired PE
         -C            append FASTA/FASTQ comment to SAM output
         -V            output the reference FASTA header in the XR tag
         -Y            use soft clipping for supplementary alignments
         -M            mark shorter split hits as secondary

         -I FLOAT[,FLOAT[,INT[,INT]]]
                       specify the mean, standard deviation (10% of the mean if absent), max
                       (4 sigma from the mean if absent) and min of the insert size distribution.
                       FR orientation only. [inferred]






References
^^^^^^^^^^

**Minimap2** https://github.com/lh3/minimap2

**BWA** http://bio-bwa.sourceforge.net/

**Samtools** http://samtools.sourceforge.net/

**QualiMap** http://qualimap.bioinfo.cipf.es/doc_html/index.html
