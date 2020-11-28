Mapping of Illumina reads to assembly (2)
-------------------------------------

This is just a loop-solution to map all samples. You have already mapped one sample, skip this for the moment and come back later, if there is time left.

A loop, to to the indexing of the consensus sequence, mapping and SAM to BAM conversion for you would look like this::

  for i in {1..5}
  do 
  bwa index ~/workdir/results_artic/barcode_0$i.consensus.fasta
  bwa mem -t 14  ~/workdir/results_artic/barcode_0$i.consensus.fasta  ~/workdir/data_artic/Illumina_0$i/B000${i}_S${i}_L001_R1_001.fastq.gz ~/workdir/data_artic/Illumina_0${i}/B000${i}_S${i}_L001_R2_001.fastq.gz | samtools view -b - | samtools sort > ~/workdir/mappings/Illumina_vs_consensus_0$i.sorted.bam
  samtools index ~/workdir/mappings/Illumina_vs_consensus_0$i.sorted.bam
  done

We will use the mappings now to call Pilon.


References
^^^^^^^^^^

**BWA** http://bio-bwa.sourceforge.net/

**samtools** http://www.htslib.org
