
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

**Minimap2** https://github.com/lh3/minimap2

**BWA** http://bio-bwa.sourceforge.net/

**Samtools** http://samtools.sourceforge.net/

**QualiMap** http://qualimap.bioinfo.cipf.es/doc_html/index.html
