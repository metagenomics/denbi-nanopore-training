
Mapping to reference
--------------------

GraphMap is a novel mapper targeted at aligning long, error-prone third-generation sequencing data.
It is designed to handle Oxford Nanopore MinION 1d and 2d reads with very high sensitivity and accuracy, and also presents a significant improvement over the state-of-the-art for PacBio read mappers.

GraphMap was also designed for ease-of-use: the default parameters can handle a wide range of read lengths and error profiles, including: Illumina, PacBio and Oxford Nanopore.

We now map the different read sets to the reference, starting with the raw 1d reads:
  graphmap align -r CXERO_10272017.fna -t 16 -C -d D1.fastq -o D1.graphmap.sam 2>&1 > D1.graphmap.sam.log
  
Nanopore sequencing data of E. Coli UTI89 generated in-house and used in the paper now available on ENA:
PRJEB9557
