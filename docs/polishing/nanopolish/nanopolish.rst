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

  nanopolish variants --threads 14 --consensus -o ~/workdir/nanopolish/polishedContig.vcf -b ~/workdir/nanopore_mapping/mapping.sorted.bam -r ~/workdir/basecall/ONT.fastq.gz -g ~/workdir/assembly/assembly.contigs.fasta | tee ~/workdir/nanopolish/nanopolish.log 2> nanopolish/nanopolish.err

This will run a few hours over night and we can get to dinner. :)

Next morning, we need to create the corrected fasta file from the generated vcf::

  nanopolish vcf2fasta -g ~/workdir/assembly/assembly.contigs.fasta ~/workdir/nanopolish/polishedContig.vcf > ~/workdir/nanopolish/polishedContig.fasta
