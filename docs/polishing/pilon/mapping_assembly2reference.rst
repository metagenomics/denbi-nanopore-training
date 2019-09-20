Mapping the polished assembly to the reference
==============================================

To evaluate the polishing of the first assembly we will now map
the polished contigs to the reference genome using LAST. 
We will re-use the index we generated earlier for the reference.
  
Now that we have an index, we can map the assembly to the reference,
convert the output to SAM and finally to BAM format::

  cd ~/workdir/pilon
  lastal ~/workdir/last_1st_assembly/Reference.db ~/workdir/pilon/pilon_round4.fasta > pilon_round4.maf
  maf-convert sam pilon_round4.maf > pilon_round4.sam
  samtools faidx ~/workdir/data/Reference.fna
  samtools view -bT ~/workdir/data/Reference.fna pilon_round4.sam > pilon_round4.bam
  samtools sort -o pilon_round4_sorted.bam pilon_round4.bam
  samtools index pilon_round4_sorted.bam
  
To look at the BAM file use::

  samtools view pilon_round4_sorted.bam | less
  
We will use a genome browser to look at the mappings. For this, you
have to 

1. open a terminal window on **your local workstation**
2. download the BAM file 
3. start `IGV: Integrative Genomics Viewer <http://www.broadinstitute.org/igv/>`_

Download the following files::

  ~/workdir/data/Reference.fna
  ~/workdir/data/Reference.fna.fai
  ~/workdir/pilon/pilon_round4_sorted.bam*

Here ist the command to open the IGV::

  /vol/cmg/bin/igv.sh
  
Now let's look at the mapped contigs:

1. Load the reference genome into IGV. Use the menu ``Genomes->Load Genome from File...`` 
2. Load the BAM file into IGV. Use menu ``File->Load from File...`` 

References
^^^^^^^^^^

**LAST** http://last.cbrc.jp/

**samtools** http://www.htslib.org

**IGV** http://www.broadinstitute.org/igv/
