Call nanopolish
---------------

Now that all pieces are together, we can call nanopolish with:

- our largest contig
- our indexed fastq files from 1D basecalling
- our mapping of 1D fastq vs. our largest contig

Put together::

  nanopolish variants --threads 14 --consensus -o ~/workdir/polishedContig.vcf -b ~/workdir/Mapping_1D_basecall_to_assembly/mapping.sorted.bam -r ~/workdir/Results/1D_basecall.fastq -g ~/workdir/canu_assembly/largestContig.fasta > nanopolish.log 2> nanopolish.err

This will run a few hours over night and we can get to dinner. :)

Next morning, we need to create the corrected fasta file from the generated vcf::

  nanopolish vcf2fasta -g ~/workdir/canu_assembly/largestContig.fasta ~/workdir/polishedContig.vcf > polishedContig.fasta
