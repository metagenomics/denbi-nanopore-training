Fast and effective quality control for MinION sequencing data
------
Developed by Rob Lanfear::

R Lanfear, M Schalamun, D Kainer, W Wang, B Schwessinger; MinIONQC: fast and simple quality control for MinION sequencing data, Bioinformatics, , bty654, https://doi.org/10.1093/bioinformatics/bty654

Script collection taht will generate a range of diagnostic plots for quality control of sequencing data from Oxford Nanopore's MinION sequencer.

MinIONQC works directly with the sequencing_summary.txt files produced by ONT's Albacore or Guppy base callers.
This allows MinIONQC for quick-and-easy comparison of data from one or multiple flowcells.

MinionQC
------

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


Inspect Data
^^^^^^^^^^^^^^
Evaluate with fastqc::
  
  cd ~/workdir
  mkdir -p ~/workdir/FastQC/1D_fastqc
  mkdir -p ~/workdir/FastQC/1D2_fastqc
  mkdir -p ~/workdir/FastQC/illumina_fastqc
  fastqc -t 14 -o ~/workdir/FastQC/1D_fastqc/ 1D_basecall.fastq
  fastqc -t 14 -o ~/workdir/FastQC/1D2_fastqc/ 1D2_basecall.fastq
  fastqc -t 14 -o ~/workdir/FastQC/illumina_fastqc/ ~/workdir/Data/Illumina/TSPf_R1.fastq.gz ~/workdir/Data/Illumina/TSPf_R2.fastq.gz
  
After that, you can load the reports in your web browser via Cloud9. Just right-click on the file in the
directory tree on the left side of your Cloud9 window and choose "Preview".
  
We will inspect the results together now ...

You should also check out the `FastQC home page <http://www.bioinformatics.babraham.ac.uk/projects/fastqc/>`_ for examples
of reports including bad data.

Handle adapter contamination
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

As we see some strange GC content at the 5' end of our nanopore reads, we can alter the way the plots are generated and turn off the grouping of reads into bins. Notice, this will generate very huge plots! To avoid this, we will first trim our reads to the first 100 base positions and do the analysis only on that::

  cd ~/workdir
  mkdir -p ~/workdir/FastQC/1D_fastqc_nogroup
  cat ~/workdir/1D_basecall.fastq  | perl -ne '{chomp; if ($_ =~ m/^@.{8}-.{4}-.{4}-.{12}/) {print $_."\n"} else {print substr($_,0,100)."\n"} }' > ~/workdir/1D_basecall_100.fastq
  fastqc -t 14 -o ~/workdir/FastQC/1D_fastqc_nogroup/ --nogroup --extract 1D_basecall_100.fastq  
  grep -A 100 "Per base sequence" ~/workdir/FastQC/1D_fastqc_nogroup/1D_basecall_100_fastqc/fastqc_data.txt 
  
So the first bases may indicate an adaptor contamination. For workflows including de novo assembly refined with nanopolish adaptor trimming is not necessary, but in other workflow scenarios this can be important to do and good there are tools which can handle this, as e.g. **porechop**.

Porechop is a tool for finding and removing adapters from Oxford Nanopore reads. Adapters on the ends of reads are trimmed off, and when a read has an adapter in its middle, it is treated as chimeric and chopped into separate reads. Porechop performs thorough alignments to effectively find adapters, even at low sequence identity::

  cd ~/workdir
  porechop -i 1D_basecall.fastq -t 14 -v 2 -o 1D_basecall.trimmed.fastq > porechop.log

Let's inspect the log file::

  cat porechop.log | more
  
So here, the following adapters were found and trimmed of in 16,500 of 20,051 cases::

  SQK-NSK007_Y_Top:     AATGTACTTCGTTCAGTTACGTATTGCT
  SQK-NSK007_Y_Bottom:  GCAATACGTAACTGAACGAAGT
  1D2_part_1_start:     GAGAGGTTCCAAGTCAGAGAGGTTCCT
  1D2_part_1_end:       AGGAACCTCTCTGACTTGGAACCTCTC
  1D2_part_2_start:     CTTCGTTCAGTTACGTATTGCTGGCGTCTGCTT
  1D2_part_2_end:       CACCCAAGCAGACGCCAGCAATACGTAACT


We will again look into the results of FastQC::

  mkdir -p ~/workdir/FastQC/1D_fastqc_trimmed
  fastqc -t 14 -o ~/workdir/FastQC/1D_fastqc_trimmed/ 1D_basecall.trimmed.fastq
  
References
^^^^^^^^^^

**FastQC** https://www.bioinformatics.babraham.ac.uk/projects/fastqc/

**Porechop** https://github.com/rrwick/Porechop
