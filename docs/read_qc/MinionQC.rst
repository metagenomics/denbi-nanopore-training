MinIONQC
------
Developed by Rob Lanfear::

  R Lanfear, M Schalamun, D Kainer, W Wang, B Schwessinger; MinIONQC: fast and simple quality control for MinION sequencing data, Bioinformatics, , bty654, https://doi.org/10.1093/bioinformatics/bty654

Script collection that will generate a range of diagnostic plots for quality control of sequencing data from Oxford Nanopore's MinION sequencer.

MinIONQC works directly with the sequencing_summary.txt files produced by ONT's Albacore or Guppy base callers.
This allows MinIONQC for quick-and-easy comparison of data from one or multiple flowcells.

Complete manual can be looked up at: https://github.com/roblanf/minion_qc

Usage::
  
 Rscript ~/MinIONQC.R --help
 Usage: /home/ubuntu/MinIONQC.R [options]

Options:
	-h, --help
		Show this help message and exit

	-i INPUT, --input=INPUT
		Input file or directory (required). Either a full path to a sequence_summary.txt file, or a full path to a directory containing one or more such files. In the latter case the directory is searched recursively.

	-o OUTPUTDIRECTORY, --outputdirectory=OUTPUTDIRECTORY
		Output directory (optional, default is the same as the input directory). If a single sequencing_summary.txt file is passed as input, then the output directory will contain just the plots associated with that file. If a directory containing more than one sequencing_summary.txt files is passed as input, then the plots will be put into sub-directories that have the same names as the parent directories of each sequencing_summary.txt file

	-q QSCORE_CUTOFF, --qscore_cutoff=QSCORE_CUTOFF
		The cutoff value for the mean Q score of a read (default 7). Used to create separate plots for reads above and below this threshold

	-p PROCESSORS, --processors=PROCESSORS
		Number of processors to use for the anlaysis (default 1). Only helps when you are analysing more than one sequencing_summary.txt file at a time

	-s SMALLFIGURES, --smallfigures=SMALLFIGURES
		TRUE or FALSE (the default). When true, MinIONQC will output smaller figures, e.g. suitable for publications or presentations. The default is to produce larger figures optimised for display on screen. Some figures just require small text, and cannot be effectively resized.


Fast and effective quality control for MinION sequencing data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Run MinIONQC on nanopore data::

  cd ~/workdir
  mkdir -p ~/workdir/MinIONQC  
  Rscript ~/MinIONQC.R -i basecall -q 12 -o MinIONQC -p 14
    
This will create several analysis plots. After that, you can load the plots in your web browser by using a file browser.
  
We will inspect the results together now ...

Again, check out the corresponding home page to learn more about all generated results: https://github.com/roblanf/minion_qc
