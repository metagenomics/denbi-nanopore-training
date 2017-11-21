Quality control by mapping
==========================

In this part of the tutorial we will look at the assemblies by mapping
the contigs of our first assembly to the reference genome using LAST. 
 
LAST is designed for comparing large datasets to each other (e.g. 
vertebrate genomes and/or large numbers of DNA reads). It can:

- Indicate the (un)ambiguity of each column in an alignment.
- Use sequence quality data in a rigorous fashion.
- Align DNA to proteins with frameshifts.
- Compare PSSMs to sequences.
- Calculate the likelihood of chance similarities between random sequences.
- Do split and spliced alignment.
- Train alignment parameters for unusual kinds of sequence (e.g. nanopore).

See the `LAST webpage <http://last.cbrc.jp/>`_ for more details.

LAST needs to build an index for the reference before we can align 
our assembly to it. This is done by the using the command ``lastdb``::

  cd ~/workdir/
  mkdir last_1st_assembly
  cd last_1st_assembly
  lastdb CXERO_10272017.db ~/Data/Reference/CXERO_10272017.fna
  
Now that we have an index, we can map the assembly to the reference::

  lastal CXERO_10272017.db ~/workdir/canu_assembly/canuAssembly.contigs.fasta > canu_1st_Assembly.maf
  
``lastal`` produces output in `MAF format
<http://genome.ucsc.edu/FAQ/FAQformat.html#format5>`_ by default. As we are going to
inspect the alignment in a genome viewer, we have to convert this into a sorted BAM file. 
LAST provides the script `maf-convert <http://last.cbrc.jp/doc/maf-convert.html>`_ 
to convert MAF to different other formats::

  maf-convert sam canu_1st_Assembly.maf > canu_1st_Assembly.sam

SAM and BAM files can be viewed and manipulated with `SAMtools <http://www.htslib.org/>`_. 
Let's first build an index for the FASTA file of the reference sequence::

  samtools faidx ~/Data/Reference/CXERO_10272017.fna

Now we can convert the SAM file into the binary BAM format and add an appropriate header to the BAM
file. After that we need to sort the alignments in the BAM file by starting position (``samtools sort``)
and index the file for fast access (``samtools index``)::

  samtools view -bT ~/Data/Reference/CXERO_10272017.fna canu_1st_Assembly.sam > canu_1st_Assembly.bam
  samtools sort -o canu_1st_Assembly_sorted.bam canu_1st_Assembly.bam
  samtools index canu_1st_Assembly_sorted.bam
  
To look at the BAM file use::

  samtools view canu_1st_Assembly_sorted.bam | less
  
We will use a genome browser to look at the mappings. For this, you
have to 

1. open a terminal window on **your local workstation**
2. download the BAM file 
3. start `IGV: Integrative Genomics Viewer <http://www.broadinstitute.org/igv/>`_

Here are the commands to copy the files and open the IGV::

  cd ~
  mkdir IGV_mappings
  cd IGV_mappings
  scp -i $PATH_TO_YOUR_SECRET_SSH_KEY_FILE ubuntu@$YOUR_OPENSTACK_INSTANCE_IP:~/Data/Reference/CXERO_10272017.fna .
  scp -i $PATH_TO_YOUR_SECRET_SSH_KEY_FILE ubuntu@$YOUR_OPENSTACK_INSTANCE_IP:~/Data/Reference/CXERO_10272017.fna.fai .
  scp -i $PATH_TO_YOUR_SECRET_SSH_KEY_FILE ubuntu@$YOUR_OPENSTACK_INSTANCE_IP:~/workdir/last_1st_assembly/canu_1st_Assembly_sorted.bam* .
  igv.sh
  
Now let's look at the mapped contigs:

1. Load the reference genome into IGV. Use the menu ``Genomes->Load Genome from File...`` 
2. Load the BAM file into IGV. Use menu ``File->Load from File...`` 

References
^^^^^^^^^^

**LAST** http://last.cbrc.jp/
**samtools** http://www.htslib.org
**IGV** http://www.broadinstitute.org/igv/
