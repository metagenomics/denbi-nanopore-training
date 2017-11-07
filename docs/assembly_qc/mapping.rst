Read Mapping
============

In this part of the tutorial we will look at the assemblies by mapping
the reads to the assembled contigs.  Different tools exists for
mapping reads to genomic sequences such as `bowtie
<http://bowtie-bio.sourceforge.net/bowtie2/index.shtml>`_ or `bwa
<http://bio-bwa.sourceforge.net/>`_. Today, we will use the tool
BBMap.

BBMap: Short read aligner for DNA and RNA-seq data. Capable of
handling arbitrarily large genomes with millions of scaffolds. Handles
Illumina, PacBio, 454, and other reads; very high sensitivity and
tolerant of errors and numerous large indels. Very fast. See the
`BBMap home page <http://sourceforge.net/projects/bbmap/>`_ for more
info.


``bbmap`` needs to build an index for the contigs sequences before it
can map the reads onto them. Here is an example command line for
mapping the reads back to the MEGAHIT assembly::

  cd /vol/spool/workdir/assembly/megahit_out

  qsub -cwd -N bbmap_index -b y \
  /usr/local/bin/bbmap.sh ref=final.contigs.fa
  
Now that we have an index, we can map the reads::

  qsub -cwd -pe multislot 12 -N bbmap -b y \
  /usr/local/bin/bbmap.sh in=../read1.fq in2=../read2.fq out=megahit.sam bamscript=sam2bam.sh threads=12
  
``bbmap`` produces output in `SAM format
<http://samtools.github.io/hts-specs/SAMv1.pdf>`_ by default, usually
you want to convert this into a sorted BAM file. ``bbmap`` creates a
shell script which can be used to convert ``bbmap``'s output into BAM
format::

  qsub -cwd -pe multislot 4 -N bbmap_sam2bam sam2bam.sh

SAM and BAM files can be viewed and manipulated with `SAMtools <http://samtools.sourceforge.net/>`_. Let's first build an index for the FASTA file::

  samtools faidx final.contigs.fa

To look at the BAM file use::

  samtools view megahit_sorted.bam | less
  
We will use a genome browser to look at the mappings. For this, you
have to 

1. open a terminal window on **your local workstation**
2. download the BAM file 
3. start `IGV: Integrative Genomics Viewer <http://www.broadinstitute.org/igv/>`_

Here are the commands to copy the files and open the IGV::

  cd ~/mg-tutorial
  scp -i PATH_TO_YOUR_SECRET_SSH_KEY_FILE ubuntu@$BIBIGRID_MASTER:workdir/assembly/megahit_out/final.contigs.fa* .
  scp -i PATH_TO_YOUR_SECRET_SSH_KEY_FILE ubuntu@$BIBIGRID_MASTER:workdir/assembly/megahit_out/*.bam* .
  scp -i PATH_TO_YOUR_SECRET_SSH_KEY_FILE ubuntu@$BIBIGRID_MASTER:workdir/assembly/megahit_out/*.gff .
  igv.sh
  
Now let's look at the mapped reads:

1. Load the contig sequences into IGV. Use the menu ``Genomes->Load Genome from File...`` 
2. Load the BAM file into IGV. Use menu ``File->Load from File...`` 
3. Load the predicted genes as another track. Use menu ``File->Load from File...`` to load the GFF file.


