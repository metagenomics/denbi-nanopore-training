Basecalling with Guppy
-------------------------


Guppy is a data processing toolkit that contains the Oxford Nanopore Technologies' basecalling algorithms, and several bioinformatic post-processing features. It is provided as binaries to run on Windows, OS X and Linux platforms, as well as being integrated with MinKNOW, the Oxford Nanopore device control software.

Early downstream analysis components such as barcoding/demultiplexing, adapter trimming and alignment are contained within Guppy. Furthermore, Guppy now performs modified basecalling (5mC, 6mA and CpG) from the raw signal data, producing an additional FAST5 file of modified base probabilities.

The command we are using for for basecalling with Guppy is::

  guppy_basecaller
  

Let's have a look at the usage message for read_fast5_basecaller.py::

  guppy_basecaller --help
  
  : Guppy Basecalling Software, (C) Oxford Nanopore Technologies, Limited. Version 3.1.5+781ed57

  Usage:

  With config file:
    guppy_basecaller -i <input path> -s <save path> -c <config file> [options]
  With flowcell and kit name:
    guppy_basecaller -i <input path> -s <save path> --flowcell <flowcell name>
      --kit <kit name>
  List supported flowcells and kits:
    guppy_basecaller --print_workflows

Beside the path of our fast5 files (-i), the basecaller requires an output path (-s) and a config file or the flowcell/kit combination. In order to get a list of possible flowcell/kit combinations and config files, we use::

  guppy_basecaller --print_workflows
  
  Available flowcell + kit combinations are:
  flowcell   kit        barcoding config_name
  FLO-MIN107 SQK-DCS108           dna_r9.5_450bps
  FLO-MIN107 SQK-DCS109           dna_r9.5_450bps
  FLO-MIN107 SQK-LRK001           dna_r9.5_450bps
  FLO-MIN107 SQK-LSK108           dna_r9.5_450bps
  FLO-MIN107 SQK-LSK109           dna_r9.5_450bps
  FLO-MIN107 SQK-LSK308           dna_r9.5_450bps
  FLO-MIN107 SQK-LSK309           dna_r9.5_450bps
  FLO-MIN107 SQK-LSK319           dna_r9.5_450bps
  FLO-MIN107 SQK-LWP001           dna_r9.5_450bps
  FLO-MIN107 SQK-PCS108           dna_r9.5_450bps
  FLO-MIN107 SQK-PCS109           dna_r9.5_450bps
  FLO-MIN107 SQK-PSK004           dna_r9.5_450bps
  FLO-MIN107 SQK-RAD002           dna_r9.5_450bps
  FLO-MIN107 SQK-RAD003           dna_r9.5_450bps
  FLO-MIN107 SQK-RAD004           dna_r9.5_450bps
  FLO-MIN107 SQK-RAS201           dna_r9.5_450bps
  FLO-MIN107 SQK-RLI001           dna_r9.5_450bps
  FLO-MIN107 VSK-VBK001           dna_r9.5_450bps
  FLO-MIN107 VSK-VSK001           dna_r9.5_450bps
  FLO-MIN107 VSK-VSK002           dna_r9.5_450bps
  FLO-MIN107 SQK-LWB001 included  dna_r9.5_450bps
  FLO-MIN107 SQK-PBK004 included  dna_r9.5_450bps
  FLO-MIN107 SQK-RAB201 included  dna_r9.5_450bps
  FLO-MIN107 SQK-RAB204 included  dna_r9.5_450bps
  FLO-MIN107 SQK-RBK001 included  dna_r9.5_450bps
  FLO-MIN107 SQK-RBK004 included  dna_r9.5_450bps
  FLO-MIN107 SQK-RLB001 included  dna_r9.5_450bps
  FLO-MIN107 SQK-RPB004 included  dna_r9.5_450bps
  FLO-MIN107 VSK-VMK001 included  dna_r9.5_450bps
  FLO-MIN107 VSK-VMK002 included  dna_r9.5_450bps
  FLO-FLG001 SQK-RNA001           rna_r9.4.1_70bps_hac
  FLO-FLG001 SQK-RNA002           rna_r9.4.1_70bps_hac
  FLO-MIN106 SQK-RNA001           rna_r9.4.1_70bps_hac
  FLO-MIN106 SQK-RNA002           rna_r9.4.1_70bps_hac
  FLO-MIN107 SQK-RNA001           rna_r9.4.1_70bps_hac
  FLO-MIN107 SQK-RNA002           rna_r9.4.1_70bps_hac
  FLO-PRO001 SQK-LSK109           dna_r9.4.1_450bps_hac_prom
  FLO-PRO001 SQK-LSK109-XL          dna_r9.4.1_450bps_hac_prom
  FLO-PRO001 SQK-DCS109           dna_r9.4.1_450bps_hac_prom
  FLO-PRO001 SQK-PCS109           dna_r9.4.1_450bps_hac_prom
  FLO-PRO001 SQK-PCB109 included  dna_r9.4.1_450bps_hac_prom
  FLO-PRO002 SQK-LSK109           dna_r9.4.1_450bps_hac_prom
  FLO-PRO002 SQK-LSK109-XL          dna_r9.4.1_450bps_hac_prom
  FLO-PRO002 SQK-DCS109           dna_r9.4.1_450bps_hac_prom
  FLO-PRO002 SQK-PCS109           dna_r9.4.1_450bps_hac_prom
  FLO-PRO002 SQK-PCB109 included  dna_r9.4.1_450bps_hac_prom
  FLO-FLG001 SQK-CAS109           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-DCS108           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-DCS109           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-LRK001           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-LSK108           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-LSK109           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-LSK109-XL          dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-LWP001           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-PCS108           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-PCS109           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-PSK004           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-RAD002           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-RAD003           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-RAD004           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-RAS201           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-RLI001           dna_r9.4.1_450bps_hac
  FLO-FLG001 VSK-VBK001           dna_r9.4.1_450bps_hac
  FLO-FLG001 VSK-VSK001           dna_r9.4.1_450bps_hac
  FLO-FLG001 VSK-VSK002           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-16S024 included  dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-PCB109 included  dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-RBK001 included  dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-RBK004 included  dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-RLB001 included  dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-LWB001 included  dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-PBK004 included  dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-RAB201 included  dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-RAB204 included  dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-RPB004 included  dna_r9.4.1_450bps_hac
  FLO-FLG001 VSK-VMK001 included  dna_r9.4.1_450bps_hac
  FLO-FLG001 VSK-VMK002 included  dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-CAS109           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-DCS108           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-DCS109           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-LRK001           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-LSK108           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-LSK109           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-LSK109-XL          dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-LWP001           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-PCS108           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-PCS109           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-PSK004           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-RAD002           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-RAD003           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-RAD004           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-RAS201           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-RLI001           dna_r9.4.1_450bps_hac
  FLO-MIN106 VSK-VBK001           dna_r9.4.1_450bps_hac
  FLO-MIN106 VSK-VSK001           dna_r9.4.1_450bps_hac
  FLO-MIN106 VSK-VSK002           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-16S024 included  dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-PCB109 included  dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-RBK001 included  dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-RBK004 included  dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-RLB001 included  dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-LWB001 included  dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-PBK004 included  dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-RAB201 included  dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-RAB204 included  dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-RPB004 included  dna_r9.4.1_450bps_hac
  FLO-MIN106 VSK-VMK001 included  dna_r9.4.1_450bps_hac
  FLO-MIN106 VSK-VMK002 included  dna_r9.4.1_450bps_hac
  FLO-PRO001 SQK-RNA002           rna_r9.4.1_70bps_hac_prom
  FLO-PRO002 SQK-RNA002           rna_r9.4.1_70bps_hac_prom


Our dataset was generated using the FLO-MIN106 flowcell, and the LSK109 kit, so we can use the dna_r9.4.1_450bps_hac model.

We need to specify the following options:

+------------------------------------------------------------------------+-------------------------+--------------------------------+
| What?                                                                  | parameter               | Our value                      |
+========================================================================+=========================+================================+
| The config file for our flowcell/kit combination                       | -c                      | dna_r9.4.1_450bps_hac_model.cfg|
+------------------------------------------------------------------------+-------------------------+--------------------------------+ 
| Compress the fastq output                                              | --compress_fastq                                         |
+------------------------------------------------------------------------+-------------------------+--------------------------------+
| The full path to the directory where the raw read files are located    | -i                      | ~/workdir/data/fast5_small     |
+------------------------------------------------------------------------+-------------------------+--------------------------------+
| The full path to the directory where the basecalled files will be saved| -s                      | ~/workdir/basecall_small/      |
+------------------------------------------------------------------------+-------------------------+--------------------------------+
| How many worker threads you are using                                  | --cpu_threads_per_caller| 14                             |
+------------------------------------------------------------------------+-------------------------+--------------------------------+
| Number of parallel basecallers to create                               | --num_callers           | 1                              |
+------------------------------------------------------------------------+-------------------------+--------------------------------+


Our complete command line is::

  guppy_basecaller --compress_fastq -i ~/workdir/data/fast5_tiny/ -s ~/workdir/basecall_tiny/ --cpu_threads_per_caller 14 --num_callers 1 -c dna_r9.4.1_450bps_hac.cfg
 
References
^^^^^^^^^^

**guppy** https://nanoporetech.com/

