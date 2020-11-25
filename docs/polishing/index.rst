Assembly polishing
==================

There are two general strategies for polishing:
1. Polish with Illumina data 
2. Polishing with Nanopore data

For the first the tool ``Pilon`` can be used. Since we don't have Illumina data for our WGS assembly, we can't do that here. We have Illumina data, for the ARTIC datasets, so we will repeat this part on the last day, where we generate consensus sequences using the ARTIC pipeline.

However, we can do the second polishing. There are 2 widely used tools to do that::

1. Nanopolish (works on raw fast5 files)
2. medaka (works with fastq files)

We only have fastq files, so we need to stick to the medaka polishing, which works better in practice most of the times anyway. 

We have used nanopolish in previous courses. If you are interested in an example call, check out the old documentation::

https://denbi-nanopore-training-course.readthedocs.io/en/latest/polishing/nanopolish/index.html

.. toctree::
   :maxdepth: 1

   medaka/index.rst

