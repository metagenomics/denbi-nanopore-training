Basecalling with Albacore
=========================

There are two commands for basecalling with Albacore available::

  read_fast5_basecaller.py
  
for linear chemistry, or::

  full_1dsq_basecaller.py
  
for 1D^2 chemistry.

Let's have a look at the usage message for `read_fast5_basecaller.py`:
```
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
```
