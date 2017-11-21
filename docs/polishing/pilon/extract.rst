Extracting first contig from Assembly
-------------------------------------

The ``variants --consensus`` option of nanopolish, which we will use later, only works on one contig. We continue to work only with the largest contig. We can do this, because we know, that this contig covers the complete reference. We are extracting the first (and largest) contig from our assembly::
  
  cd ~/workdir/
  samtools faidx ~/workdir/canu_assembly/canuAssembly.contigs.fasta tig00000001 > ~/workdir/canu_assembly/largestContig.fasta


References
^^^^^^^^^^

**samtools** http://www.htslib.org
