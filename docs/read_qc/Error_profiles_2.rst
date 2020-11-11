
Generating Error Profiles
-------------------------

Inferring error profiles using samtools
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The command to run samtools stats is quite simple::

  samtools stats ~/workdir/mappings/basecall_tiny_porechopped_<number>_vs_wuhan.sorted.bam >  ~/workdir/mappings/basecall_tiny_porechopped_<number>_vs_wuhan.stats

and for Illumina::

  samtools stats -@ 14 ~/workdir/mappings/illumina_vs_wuhan.sorted.bam >  ~/workdir/mappings/illumina_vs_wuhan.stats
  
Inspect the files with::

  less ~/workdir/mappings/basecall_tiny_porechopped_<number>_vs_wuhan.stats
  less ~/workdir/mappings/illumina_vs_wuhan.stats

Enhanced mapping statistics
^^^^^^^^^^^^^^^^^^^^^^^^^^^

To get a more in depth info on the actual accuracy of the data at hand, including the genome coverage, we're going to use a more comprehensive and interactive software comparable to FastQC which is called **Qualimap**. Qualimap has a tool "bamqc" for statistics on BAM files::


	usage: qualimap bamqc -bam <arg> [-c] [-gd <arg>] [-gff <arg>] [-hm <arg>] [-ip]
	       [-nr <arg>] [-nt <arg>] [-nw <arg>] [-oc <arg>] [-os] [-outdir <arg>]
	       [-outfile <arg>] [-outformat <arg>] [-p <arg>] [-sd] [-sdmode <arg>]
	 -bam <arg>                           Input mapping file in BAM format
	 -c,--paint-chromosome-limits         Paint chromosome limits inside charts
	 -gd,--genome-gc-distr <arg>          Species to compare with genome GC
					      distribution. Possible values: HUMAN -
					      hg19; MOUSE - mm9(default), mm10
	 -gff,--feature-file <arg>            Feature file with regions of interest in
					      GFF/GTF or BED format
	 -hm <arg>                            Minimum size for a homopolymer to be
					      considered in indel analysis (default is
					      3)
	 -ip,--collect-overlap-pairs          Activate this option to collect statistics
					      of overlapping paired-end reads
	 -nr <arg>                            Number of reads analyzed in a chunk
					      (default is 1000)
	 -nt <arg>                            Number of threads (default is 28)
	 -nw <arg>                            Number of windows (default is 400)
	 -oc,--output-genome-coverage <arg>   File to save per base non-zero coverage.
					      Warning: large files are expected for
					      large genomes
	 -os,--outside-stats                  Report information for the regions outside
					      those defined by feature-file  (ignored
					      when -gff option is not set)
	 -outdir <arg>                        Output folder for HTML report and raw
					      data.
	 -outfile <arg>                       Output file for PDF report (default value
					      is report.pdf).
	 -outformat <arg>                     Format of the output report (PDF, HTML or
					      both PDF:HTML, default is HTML).
	 -p,--sequencing-protocol <arg>       Sequencing library protocol:
					      strand-specific-forward,
					      strand-specific-reverse or
					      non-strand-specific (default)
	 -sd,--skip-duplicated                Activate this option to skip duplicated
					      alignments from the analysis. If the
					      duplicates are not flagged in the BAM
					      file, then they will be detected by
					      Qualimap and can be selected for skipping.
	 -sdmode,--skip-dup-mode <arg>        Specific type of duplicated alignments to
					      skip (if this option is activated).
					      0 : only flagged duplicates (default)
					      1 : only estimated by Qualimap
					      2 : both flagged and estimated



	Special arguments: 

	    --java-mem-size  Use this argument to set Java memory heap size. Example:
			     qualimap bamqc -bam very_large_alignment.bam --java-mem-size=4G

Run::

  qualimap bamqc
  
on the mapping files now (*.sorted.bam - Illumina and Nanopore mappings). Use the following options:;

  -nt 14
  -nw 50000
  -bam <input file>
  -outdir <output directory>
  
The output folders should be named::
  
  ~/workdir/mappings/basecall_tiny_porechopped_<number>_vs_wuhan_qualimap/
  and
  ~/workdir/mappings/illumina_vs_wuhan_qualimap/

Help is available on the next page.


References
^^^^^^^^^^

**Samtools** http://samtools.sourceforge.net/

**QualiMap** http://qualimap.bioinfo.cipf.es/doc_html/index.html
