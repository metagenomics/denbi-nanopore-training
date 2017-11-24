Annotating the genome with prokka
=================================

The multiplex capability and high yield of current day DNA-sequencing instruments has made bacterial whole genome sequencing a routine affair. The subsequent de novo assembly of reads into contigs has been well addressed. The final step of annotating all relevant genomic features on those contigs can be achieved slowly using existing web- and email-based systems, but these are not applicable for sensitive data or integrating into computational pipelines. Prokka is a command line software tool to fully annotate a draft bacterial genome in about 10 min on a typical desktop computer. It produces standards-compliant output files for further analysis or viewing in genome browsers.

In order to call prokka, we first need to shorten the name of our largest contig (the genome sequence) because it has gotten quite long due to several pilon rounds. We use a simple shell command to do that::

  cat ~/workdir/Annotation/circlator.circularise.fasta | sed -e 's/>tig.*/>genome_sequence/g' > ~/workdir/Annotation/genome.fasta

Then we call Prokka with the genome sequence and specify an output directory::

  prokka ~/workdir/Annotation/genome.fasta --outdir ~/workdir/Annotation/prokka/

We will use a genome browser to look at the annotated genome. For this, you have to

1. open a terminal window on **your local workstation**
2. download the prokka files
3. start `IGV: Integrative Genomics Viewer`_

Here are the commands to copy the files and open the IGV::

  cd ~
  mkdir prokka_genome
  cd prokka_genome
  scp -r -i $PATH_TO_YOUR_SECRET_SSH_KEY_FILE ubuntu@$YOUR_OPENSTACK_INSTANCE_IP:~/workdir/Annotation/prokka/ .
  /vol/cmg/bin/igv.sh
  
Now let's look at the annoated genome in IGV. Use the menu ``Genomes->Load Genome from File...``




References
^^^^^^^^^^

**prokka** http://www.vicbioinformatics.com/software.prokka.shtml

**IGV** http://www.broadinstitute.org/igv/
