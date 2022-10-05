Quality control by mapping(2)
==========================

Map the data to the Wuhan reference::

  minimap2 -a -t 14 ~/workdir/wuhan.fasta ~/workdir/assembly/assembly_wgs/assembly.contigs.fasta | samtools view -b - | samtools sort - > ~/workdir/mappings/assembly_wgs_vs_wuhan.sorted.bam
  
Create the index::

  samtools index ~/workdir/mappings/assembly_wgs_vs_wuhan.sorted.bam
  
Load GenomeView with::

  java -jar ~/genomeview-N42.jar
  
Load the wuhan reference and the mapping and look at the data - is it more equally distributed?


In the next step, we perform the assembly with canu and the WGS dataset.References
^^^^^^^^^^


**Minimap2** https://github.com/lh3/minimap2

**samtools** http://www.htslib.org

**IGV** http://www.broadinstitute.org/igv/
