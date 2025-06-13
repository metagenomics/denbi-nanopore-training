Basecalling 
===========

Basecalling with Albacore
-------------------------

Albacore is a data processing pipeline that provides the Oxford Nanopore basecalling algorithms, and several post-processing steps. It is run from the command line on Windows, Mac OS X, and multiple Linux platforms. A selection of configuration files allow basecalling DNA libraries made with the current range of sequencing kits and flow cells.

The Albacore pipeline contains:

1. Basecalling: a similar implementation of algorithms as found in MinKNOW basecalling. However, it also contains configuration files for basecalling chemistry that is not currently handled by MinKNOW, e.g. 1D2 reads.

2. Calibration Strand Detection: Reads are aligned against a calibration strand reference via the integrated minimap2 aligner. Calibration strands serve as a quality control for pore and experiment. If the current read is identified as a calibration strand, no barcoding or alignment steps are performed.

3. Barcoding/Demultiplexing: The beginning and the end of each strand are aligned against the barcodes currently provided by Oxford Nanopore Technologies. The reads are demultiplexed by the barcoding results.

4. Alignment: The user can provide a reference file in FASTA, lastdb or minimap2 index format. If so, the reads are aligned against this reference via the integrated minimap2 aligner.


There are two commands for basecalling with Albacore which we will use available - for linear chemistry::

  read_fast5_basecaller.py
  
or, for 1D² chemistry::

  full_1dsq_basecaller.py
  
The ``full_1dsq_basecaller.py`` basically just wraps the two successive commands, which could also be used independently::
  
  read_fast5_basecaller.py
  paired_read_basecaller.py



Let's have a look at the usage message for read_fast5_basecaller.py::

  read_fast5_basecaller.py --help
  usage: read_fast5_basecaller.py [-h] [-l] [-v] [-i INPUT] -t WORKER_THREADS -s
                                  SAVE_PATH [-f FLOWCELL] [-k KIT] [--barcoding]
                                  [-c CONFIG] [-d DATA_PATH] [-b] [-r]
                                  [-n FILES_PER_BATCH_FOLDER] [-o OUTPUT_FORMAT]
                                  [-q READS_PER_FASTQ_BATCH]
                                  [--disable_filtering]

  ONT Albacore Sequencing Pipeline Software

  optional arguments:
    -h, --help            show this help message and exit
    -l, --list_workflows  List standard flowcell / kit combinations.
    -v, --version         Print the software version.
    -i INPUT, --input INPUT
                          Folder containing read fast5 files (if not present,
                          will expect file names on stdin).
    -t WORKER_THREADS, --worker_threads WORKER_THREADS
                          Number of worker threads to use.
    -s SAVE_PATH, --save_path SAVE_PATH
                          Path to save output.
    -f FLOWCELL, --flowcell FLOWCELL
                          Flowcell used during the sequencing run.
    -k KIT, --kit KIT     Kit used during the sequencing run.
    --barcoding           Search for barcodes to demultiplex sequencing data.
    -c CONFIG, --config CONFIG
                          Optional configuration file to use.
    -d DATA_PATH, --data_path DATA_PATH
                          Optional path to model files.
    -b, --debug           Output additional debug information to the log.
    -r, --recursive       Recurse through subfolders for input data files.
    -n FILES_PER_BATCH_FOLDER, --files_per_batch_folder FILES_PER_BATCH_FOLDER
                          Maximum number of files in each batch subfolder. Set
                          to 0 to disable batch subfolders.
    -o OUTPUT_FORMAT, --output_format OUTPUT_FORMAT
                          desired output format, can be fastq,fast5 or only one
                          of these.
    -q READS_PER_FASTQ_BATCH, --reads_per_fastq_batch READS_PER_FASTQ_BATCH
                          number of reads per FastQ batch file. Set to 1 to
                          receive one reads per file and file names which
                          include the read ID. Set to 0 to have all reads per
                          run ID written to one file.
    --disable_filtering   Disable filtering into pass/fail folders

We can get a list of supported flowcell + kit combinations by::

  read_fast5_basecaller.py -l
  Parsing config files in /opt/albacore.
  Available flowcell + kit combinations are:
  flowcell    kit         barcoding  config file
  FLO-MIN106  SQK-DCS108             r94_450bps_linear.cfg
  FLO-MIN106  SQK-LSK108             r94_450bps_linear.cfg
  FLO-MIN106  SQK-LWB001  included   r94_450bps_linear.cfg
  FLO-MIN106  SQK-LWP001             r94_450bps_linear.cfg
  FLO-MIN106  SQK-PCS108             r94_450bps_linear.cfg
  FLO-MIN106  SQK-RAB201  included   r94_450bps_linear.cfg
  FLO-MIN106  SQK-RAD002             r94_450bps_linear.cfg
  FLO-MIN106  SQK-RAD003             r94_450bps_linear.cfg
  FLO-MIN106  SQK-RAS201             r94_450bps_linear.cfg
  FLO-MIN106  SQK-RBK001  included   r94_450bps_linear.cfg
  FLO-MIN106  SQK-RLB001  included   r94_450bps_linear.cfg
  FLO-MIN106  SQK-RLI001             r94_450bps_linear.cfg
  FLO-MIN106  SQK-RNA001             r94_70bps_rna_linear.cfg
  FLO-MIN106  VSK-VBK001             r94_450bps_linear.cfg
  FLO-MIN107  SQK-DCS108             r95_450bps_linear.cfg
  FLO-MIN107  SQK-LSK108             r95_450bps_linear.cfg
  FLO-MIN107  SQK-LWB001  included   r95_450bps_linear.cfg
  FLO-MIN107  SQK-LWP001             r95_450bps_linear.cfg
  FLO-MIN107  SQK-PCS108             r95_450bps_linear.cfg
  FLO-MIN107  SQK-RAB201  included   r95_450bps_linear.cfg
  FLO-MIN107  SQK-RAD002             r95_450bps_linear.cfg
  FLO-MIN107  SQK-RAD003             r95_450bps_linear.cfg
  FLO-MIN107  SQK-RAS201             r95_450bps_linear.cfg
  FLO-MIN107  SQK-RBK001  included   r95_450bps_linear.cfg
  FLO-MIN107  SQK-RLB001  included   r95_450bps_linear.cfg
  FLO-MIN107  SQK-RLI001             r95_450bps_linear.cfg
  FLO-MIN107  SQK-RNA001             r94_70bps_rna_linear.cfg
  FLO-MIN107  VSK-VBK001             r95_450bps_linear.cfg

We need to specify at least the following options:

+------------------------------------------------------------------------+-----------+------------------+
| What?                                                                  | parameter | Our value        |
+========================================================================+===========+==================+
| The flow cell version that was used                                    | -f        | FLO-MIN107       |
+------------------------------------------------------------------------+-----------+------------------+
|The sequencing kit version that was used                                | -k        | SQK-LSK308       |
+------------------------------------------------------------------------+-----------+------------------+
| Which output file type you want (fast5, FASTQ, or both)                | -o        | fastq            |
+------------------------------------------------------------------------+-----------+------------------+
| The full path to the directory where the raw read files are located    | -i        | ~/Nanopore_small |
+------------------------------------------------------------------------+-----------+------------------+
| The full path to the directory where the basecalled files will be saved| -s        | ~/1D_basecall    |
+------------------------------------------------------------------------+-----------+------------------+
| How many worker threads you are using                                  | -t        | 16               |
+------------------------------------------------------------------------+-----------+------------------+
| Number of reads per FASTQ batch file                                   | -q        | 100000           |
+------------------------------------------------------------------------+-----------+------------------+

Our complete command line is::

  read_fast5_basecaller.py -f FLO-MIN107 -k SQK-LSK308 -t 16 -s ~/workdir/1D_basecall_small -o fastq -q 100000 -i ~/workdir/Nanopore_small/
  
and similar for the 1D² basecalling::
  
  full_1dsq_basecaller.py -f  FLO-MIN107 -k SQK-LSK308 -t 16 -s ~/workdir/1D2_basecall_small -o fastq -q 100000 -i ~/workdir/Nanopore_small/
  

Inspect the output
------------------

Both directories contain a number of fastq files::

  ls -lh ~/workdir/1D_basecall_small/workspace/pass/
  
  total 12M
  -rw-rw-r-- 1 ubuntu ubuntu 4.6M Nov 13 10:17 fastq_runid_04d71dafbed4e1a2c29d48873533c94070985063_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu  98K Nov 13 10:17 fastq_runid_307482bb8322e11a4f92efefd01364754f9c271f_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 664K Nov 13 10:17 fastq_runid_492de34daf0e1e3648eed3c976ecf01b9ae1a60f_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 3.6M Nov 13 10:17 fastq_runid_940cafc8dbea461f32589d22e3095264700230fb_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 1.4M Nov 13 10:17 fastq_runid_cdd5fefcf4478e23e0628e437f145a503cffa888_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 865K Nov 13 10:17 fastq_runid_fa18a6a6c046ba9c4e91a6381be34a7eb06afbff_0.fastq

The D1^2 basecalling also creates additional fast5 data in the workspace. Keep that in mind, when disk space is limited. ::

  ls -lh ~/workdir/1D2_basecall_small/workspace/
  
  total 13M
  drwxrwxr-x 2 ubuntu ubuntu 144K Nov 13 10:19 0   <-- additional fast5 data
  -rw-rw-r-- 1 ubuntu ubuntu 4.6M Nov 13 10:19 fastq_runid_04d71dafbed4e1a2c29d48873533c94070985063_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 101K Nov 13 10:19 fastq_runid_307482bb8322e11a4f92efefd01364754f9c271f_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 778K Nov 13 10:19 fastq_runid_492de34daf0e1e3648eed3c976ecf01b9ae1a60f_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 4.2M Nov 13 10:19 fastq_runid_940cafc8dbea461f32589d22e3095264700230fb_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 1.6M Nov 13 10:19 fastq_runid_cdd5fefcf4478e23e0628e437f145a503cffa888_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 961K Nov 13 10:19 fastq_runid_fa18a6a6c046ba9c4e91a6381be34a7eb06afbff_0.fastq

The workspace directory above contains the 1D basecalling, whereas the 1D² basecalling is located in::

  ls -lh ~/workdir/1D2_basecall_small/1dsq_analysis/workspace/pass/

  total 1180
  -rw-rw-r-- 1 ubuntu ubuntu 559842 Nov 13 10:21 fastq_runid_04d71dafbed4e1a2c29d48873533c94070985063_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu  61776 Nov 13 10:21 fastq_runid_492de34daf0e1e3648eed3c976ecf01b9ae1a60f_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu 447404 Nov 13 10:22 fastq_runid_940cafc8dbea461f32589d22e3095264700230fb_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu  98055 Nov 13 10:22 fastq_runid_cdd5fefcf4478e23e0628e437f145a503cffa888_0.fastq
  -rw-rw-r-- 1 ubuntu ubuntu  31740 Nov 13 10:22 fastq_runid_fa18a6a6c046ba9c4e91a6381be34a7eb06afbff_0.fastq

The results with complete data
------------------------------

We have precomputed the D1 and D1² basecalling with the complete basecalling for you to save time, please copy that data into your working directory::

  cp -r ~/Results/1D_basecall/ ~/workdir/
  cp -r ~/Results/1D2_basecall/ ~/workdir/

Merge fastqs
------------

To make life easier for future computations, we will merge the fastq files into one::

  cat ~/workdir/1D_basecall_small/workspace/pass/*.fastq > ~/workdir/1D_basecall_small.fastq
  cat ~/workdir/1D2_basecall_small/1dsq_analysis/workspace/pass/*.fastq > ~/workdir/1D2_basecall_small.fastq
  cat ~/workdir/1D_basecall/workspace/pass/*.fastq > ~/workdir/1D_basecall.fastq
  cat ~/workdir/1D2_basecall/1dsq_analysis/workspace/pass/*.fastq > ~/workdir/1D2_basecall.fastq



