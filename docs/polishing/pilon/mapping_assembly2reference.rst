Mapping the polished assembly to the reference
==============================================

To evaluate the polishing of the first assembly we will now map
the polished contigs to the reference genome using LAST. 
We will re-use the index we generated earlier for the reference.
  
Now that we have an index, we can map the assembly to the reference,
convert the output to SAM and finally to BAM format::

  cd ~/workdir/Pilon
  lastal ~/workdir/last_1st_assembly/CXERO_10272017.db ~/workdir/Pilon/Pilon_round4.fasta > Pilon_round4.maf
  maf-convert sam Pilon_round4.maf > Pilon_round4.sam
  samtools faidx ~/workdir/Data/Reference/CXERO_10272017.fna
  samtools view -bT ~/workdir/Data/Reference/CXERO_10272017.fna Pilon_round4.sam > Pilon_round4.bam
  samtools sort -o Pilon_round4_sorted.bam Pilon_round4.bam
  samtools index Pilon_round4_sorted.bam
  
To look at the BAM file use::

  samtools view Pilon_round4_sorted.bam | less
  
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
  scp -i $PATH_TO_YOUR_SECRET_SSH_KEY_FILE ubuntu@$YOUR_OPENSTACK_INSTANCE_IP:~/workdir/Pilon/Pilon_round4_sorted.bam* .
  /vol/cmg/bin/igv.sh
  
Now let's look at the mapped contigs:

1. Load the reference genome into IGV. Use the menu ``Genomes->Load Genome from File...`` 
2. Load the BAM file into IGV. Use menu ``File->Load from File...`` 

References
^^^^^^^^^^

**LAST** http://last.cbrc.jp/

**samtools** http://www.htslib.org

**IGV** http://www.broadinstitute.org/igv/
