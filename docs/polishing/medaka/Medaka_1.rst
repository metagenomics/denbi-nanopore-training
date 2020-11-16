Polishing with medaka
-----------------------

Medaka is a tool to create a consensus sequence of nanopore sequencing data. This task is performed using neural networks applied a pileup of individual sequencing reads against a draft assembly. It outperforms graph-based methods operating on basecalled data, and can be competitive with state-of-the-art signal-based methods whilst being much faster.

In earlier courses, we used nanopolish for polishing but it is outperformed by medaka in both runtime and accuracy.

As input medaka accepts reads in either a .fasta or a .fastq file. It requires a draft assembly as a .fasta.

Check the usage of medaka_consensus::

  usage: medaka consensus [-h] [--debug | --quiet] [--batch_size BATCH_SIZE] [--chunk_len CHUNK_LEN] [--chunk_ovlp CHUNK_OVLP] [--regions REGIONS [REGIONS ...]] [--model MODEL] [--RG READGROUP]
                          [--threads THREADS] [--check_output] [--save_features] [--tag_name TAG_NAME] [--tag_value TAG_VALUE] [--tag_keep_missing]
                          bam output

  positional arguments:
    bam                   Input alignments.
    output                Output file.

  optional arguments:
    -h, --help            show this help message and exit
    --debug               Verbose logging of debug information. (default: 20)
    --quiet               Minimal logging; warnings only). (default: 20)
    --batch_size BATCH_SIZE
                          Inference batch size. (default: 100)
    --chunk_len CHUNK_LEN
                          Chunk length of samples. (default: 10000)
    --chunk_ovlp CHUNK_OVLP
                          Overlap of chunks. (default: 1000)
    --regions REGIONS [REGIONS ...]
                          Genomic regions to analyse, or a bed file. (default: None)
    --model MODEL         Model to use. {r103_min_high_g345, r103_min_high_g360, r103_prom_high_g360, r103_prom_snp_g3210, r103_prom_variant_g3210, r10_min_high_g303, r10_min_high_g340, r941_min_fast_g303,
                          r941_min_high_g303, r941_min_high_g330, r941_min_high_g340_rle, r941_min_high_g344, r941_min_high_g351, r941_min_high_g360, r941_prom_fast_g303, r941_prom_high_g303,
                          r941_prom_high_g330, r941_prom_high_g344, r941_prom_high_g360, r941_prom_high_g4011, r941_prom_snp_g303, r941_prom_snp_g322, r941_prom_snp_g360, r941_prom_variant_g303,
                          r941_prom_variant_g322, r941_prom_variant_g360} (default: r941_min_high_g360)
    --threads THREADS     Number of threads used by inference. (default: 1)
    --check_output        Verify integrity of output file after inference. (default: False)
    --save_features       Save features with consensus probabilities. (default: False)

  read group:
    Filtering alignments the read group (RG) tag, expected to be string value.

    --RG READGROUP        Read group to select. (default: None)

  filter tag:
    Filtering alignments by an integer valued tag.

    --tag_name TAG_NAME   Two-letter tag name. (default: None)
    --tag_value TAG_VALUE
                          Value of tag. (default: None)
    --tag_keep_missing    Keep alignments when tag is missing. (default: False)



For comparison, we run medaka on our inital assembly and on the one polished with racon.
We use the model r941_min_high_g360 (for R941 flowcell, MinION model, high accuracy, basecalled with guppy version3.60 - since this is the highest version available; we used guppy version 4.15). So we can call medaka on the racon polished assembly with::

  medaka_consensus -t 14 -m r941_min_high_g360 -i ~/workdir/data_wgs/Cov2_HK_WGS_small_porechopped.fastq.gz -d ~/workdir/assembly/assembly_wgs/racon.fasta -o ~/assembly/assembly_wgs/medaka
    
Next, we are going to have a short look on assembly results.


References
^^^^^^^^^^

**medaka** https://github.com/nanoporetech/medaka
