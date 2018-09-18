Assembly evaluation with QUAST
==============================

QUAST stands for QUality ASsessment Tool. The tool evaluates genome
assemblies by computing various metrics.  You can find all project
news and the latest version of the tool at `sourceforge
<http://sourceforge.net/projects/quast>`_.  QUAST utilizes MUMmer,
GeneMarkS, GeneMark-ES, GlimmerHMM, and GAGE. 

To call ``quast.py`` we have to provide a reference genome and one or more assemblies. We are giving both, our assembly with the reduced dataset as well as our assembly with the complete dataset as input. For comparison, we also include an Illumina assembly which has been precomputed with spades. The reference is usually not available, of course::

  quast.py -t 14 -o ~/workdir/quast_canu_assembly -R ~/workdir/Data/Reference/CXERO_10272017.fna ~/workdir/canu_assembly/canuAssembly.contigs.fasta ~/workdir/canu_assembly_small/canuAssembly.contigs.fasta ~/workdir/Results/Illumina_assembly_with_spades/contigs.fasta

QUAST generates HTML reports including a number of interactive graphics. To access these reports, open them using your Cloud9 Interface.

References
^^^^^^^^^^

**quast** http://sourceforge.net/projects/quast
