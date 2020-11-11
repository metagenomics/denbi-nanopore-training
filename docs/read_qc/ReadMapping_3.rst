
Read mapping to a reference (3)
-------------------------

SAM to BAM conversion with samtools
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To create a sorted BAM file from a SAM file we can use::

  samtools view -b ~/workdir/mappings/basecall_tiny_porechopped_<number>_vs_wuhan.sam | samtools sort - > ~/workdir/mappings/basecall_tiny_porechopped_<number>_vs_wuhan.sorted.bam
  
And for the Illumina data::

  samtools view -b ~/workdir/mappings/illumina_vs_wuhan.sam | samtools sort - > ~/workdir/mappings/illumina_vs_wuhan.sorted.bam
  

Then create indizes on the sorted BAM files::

  samtools index ~/workdir/mappings/basecall_tiny_porechopped_<number>_vs_wuhan.sorted.bam
  and 
  samtools index ~/workdir/mappings/illumina_vs_wuhan.sorted.bam
  
  

Inspect the results using GenomeView
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Then start the genome browser GenomeView::

  java -jar ~/genomeview-N42.jar

Load the Wuhan-Reference and the mappings (the sorted BAM files) and inspect the mapping results. Have a look on the mapping quality in particular (Zoom in to see mismatches in alignment). You can load data with: File->Load data... then choose the appropriate files.

To see not only a fraction of the mappings go to File->Configuration->Short reads and change the "Maxiumum display depth of stacked reads" to a larger value.

References
^^^^^^^^^^


**samtools** http://www.htslib.org

**GenomeView** https://genomeview.org/
