Call nanopolish
---------------

Now that all pieces are together, we can call nanopolish with:

- our assembly
- our indexed fastq files from 1D basecalling
- our mapping of 1D fastq vs. our assembly

Create a directory first::

  cd ~/workdir
  mkdir nanopolish

Call nanopolish::

  python /usr/local/lib/nanopolish/scripts/nanopolish_makerange.py ~/workdir/assembly/assembly.contigs.fasta | parallel --results nanopolish.results -P 7 nanopolish variants --consensus -o ~/workdir/nanopolish/polished.{1}.vcf -w {1} -r ~/workdir/basecall/ONT.fastq.gz -b ~/workdir/nanopore_mapping/mapping.sorted.bam -g ~/workdir/assembly/assembly.contigs.fasta -t 2

This will run a few hours over night and we can get to dinner. :)

Next morning, we need to create the corrected fasta file from the generated vcf::

  nanopolish vcf2fasta -g ~/workdir/assembly/assembly.contigs.fasta ~/workdir/nanopolish/polished.*.vcf > ~/workdir/nanopolish/polished_genome.fasta
