Polishing with medaka
-----------------------

Medaka is a tool to create a consensus sequence of nanopore sequencing data. This task is performed using neural networks applied a pileup of individual sequencing reads against a draft assembly. It outperforms graph-based methods operating on basecalled data, and can be competitive with state-of-the-art signal-based methods whilst being much faster.

In earlier courses, we used nanopolish for polishing but it is outperformed by medaka in both runtime and accuracy.

As input medaka accepts a sorted and indexed BAM mapping file. It requires a draft assembly as a .fasta.

Medaka hast 3 steps / subtools:

 mini_align (basically runs a minimap2 mapping)
 medaka consensus (generates a consensus, you can do that for subparts of the assembly to improve runtime)
 medaka stitch (to stitch the subparts together, or generate a fasta from the results from medaka consensus)

However, for smaller assemblies, we can just use ``medaka_consensus`` that performs all the steps above::

medaka 1.2.0
------------

Assembly polishing via neural networks. The input assembly should be
preprocessed with racon.

medaka_consensus [-h] -i <fastx>

    -h  show this help text.
    -i  fastx input basecalls (required).
    -d  fasta input assembly (required).
    -o  output folder (default: medaka).
    -g  don't fill gaps in consensus with draft sequence.
    -m  medaka model, (default: r941_min_high_g360).
        Available: r103_min_high_g345, r103_min_high_g360, r103_prom_high_g360, r103_prom_snp_g3210, r103_prom_variant_g3210, r10_min_high_g303, r10_min_high_g340, r941_min_fast_g303, r941_min_high_g303, r941_min_high_g330, r941_min_high_g340_rle, r941_min_high_g344, r941_min_high_g351, r941_min_high_g360, r941_prom_fast_g303, r941_prom_high_g303, r941_prom_high_g330, r941_prom_high_g344, r941_prom_high_g360, r941_prom_high_g4011, r941_prom_snp_g303, r941_prom_snp_g322, r941_prom_snp_g360, r941_prom_variant_g303, r941_prom_variant_g322, r941_prom_variant_g360.
        Alternatively a .hdf file from 'medaka train'.
    -f  Force overwrite of outputs (default will reuse existing outputs).
    -t  number of threads with which to create features (default: 1).
    -b  batchsize, controls memory use (default: 100).

-i must be specified.


We need to define the following parameters::
  -i <input fastq>
  -d <racon reference assembly>
  -o <output folder>, should be: ~/workdir/assembly/assembly_wgs/medaka/
  -t <threads>
  -m <the appropriate medaka model>
  
The model are named with the following scheme::

  {pore}_{device}_{caller variant}_{caller version}
  
Our pore is r941, the device is MinION (min), we take the high-accuracy model (high), and our guppy version was 4.15. Choose the model, that is closest to that basecaller version.


If you are stuck, get help on the next page.


References
^^^^^^^^^^

**medaka** https://github.com/nanoporetech/medaka
