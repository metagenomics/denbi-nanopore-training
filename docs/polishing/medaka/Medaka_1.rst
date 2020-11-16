Polishing with medaka
-----------------------

Medaka is a tool to create a consensus sequence of nanopore sequencing data. This task is performed using neural networks applied a pileup of individual sequencing reads against a draft assembly. It outperforms graph-based methods operating on basecalled data, and can be competitive with state-of-the-art signal-based methods whilst being much faster.

In earlier courses, we used nanopolish for polishing but it is outperformed by medaka in both runtime and accuracy.

As input medaka accepts reads in either a .fasta or a .fastq file. It requires a draft assembly as a .fasta.

Check the usage of medaka_consensus::

  medaka_consensus [-h] -i <fastx>

    -h  show this help text.
    -i  fastx input basecalls (required).
    -d  fasta input assembly (required). 
    -o  output folder (default: medaka).
    -m  medaka model, (default: r941_min_high).
        Available: r941_trans, r941_flip213, r941_flip235, r941_min_fast, r941_min_high, r941_prom_fast, r941_prom_high.
        Alternatively a .hdf file from 'medaka train'. 
    -t  number of threads with which to create features (default: 1).
    -b  batchsize, controls memory use (default: 200).

  -i must be specified.


For comparison, we run medaka on our inital assembly and on the one polished with racon.
We use the model r941_min_high. So we can call medaka with::

  medaka_consensus -i basecall/basecall_trimmed.fastq.gz -d assembly/assembly.contigs.fasta -o medaka -t 14 -m r941_min_high
  
To run medaka on the racon polished assembly::

  medaka_consensus -i basecall/basecall_trimmed.fastq.gz -d racon/racon.fasta -o racon_medaka -t 14 -m r941_min_high

Next, we are going to have a short look on assembly results and further polish with pilon.


References
^^^^^^^^^^

**medaka** https://github.com/nanoporetech/medaka
