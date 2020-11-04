Basecalling with Guppy
-------------------------


Guppy is a data processing toolkit that contains the Oxford Nanopore Technologies' basecalling algorithms, and several bioinformatic post-processing features. It is provided as binaries to run on Windows, OS X and Linux platforms, as well as being integrated with MinKNOW, the Oxford Nanopore device control software.

Early downstream analysis components such as barcoding/demultiplexing, adapter trimming and alignment are contained within Guppy. Furthermore, Guppy now performs modified basecalling (5mC, 6mA and CpG) from the raw signal data, producing an additional FAST5 file of modified base probabilities.

The command we are using for for basecalling with Guppy is::

  guppy_basecaller_cpu
  

Let's have a look at the usage message for guppy_basecaller_cpu::

  guppy_basecaller_cpu --help
  
  : Guppy Basecalling Software, (C) Oxford Nanopore Technologies, Limited. Version 3.1.5+781ed57

  Usage:

  With config file:
    guppy_basecaller_cpu -i <input path> -s <save path> -c <config file> [options]
  With flowcell and kit name:
    guppy_basecaller_cpu -i <input path> -s <save path> --flowcell <flowcell name>
      --kit <kit name>
  List supported flowcells and kits:
    guppy_basecaller_cpu --print_workflows

Beside the path of our fast5 files (-i), the basecaller requires an output path (-s) and a config file or the flowcell/kit combination. In order to get a list of possible flowcell/kit combinations and config files, we use::

  guppy_basecaller_cpu --print_workflows
  
  flowcell   kit        barcoding config_name
  FLO-MIN110 SQK-CAS109           dna_r10_450bps_hac
  FLO-MIN110 SQK-CS9109           dna_r10_450bps_hac
  FLO-MIN110 SQK-DCS108           dna_r10_450bps_hac
  FLO-MIN110 SQK-DCS109           dna_r10_450bps_hac
  FLO-MIN110 SQK-LRK001           dna_r10_450bps_hac
  FLO-MIN110 SQK-LSK108           dna_r10_450bps_hac
  FLO-MIN110 SQK-LSK109           dna_r10_450bps_hac
  FLO-MIN110 SQK-LSK109-XL          dna_r10_450bps_hac
  FLO-MIN110 SQK-LSK110           dna_r10_450bps_hac
  FLO-MIN110 SQK-LWP001           dna_r10_450bps_hac
  FLO-MIN110 SQK-PCS108           dna_r10_450bps_hac
  FLO-MIN110 SQK-PCS109           dna_r10_450bps_hac
  FLO-MIN110 SQK-PRC109           dna_r10_450bps_hac
  FLO-MIN110 SQK-PSK004           dna_r10_450bps_hac
  FLO-MIN110 SQK-RAD002           dna_r10_450bps_hac
  FLO-MIN110 SQK-RAD003           dna_r10_450bps_hac
  FLO-MIN110 SQK-RAD004           dna_r10_450bps_hac
  FLO-MIN110 SQK-RAS201           dna_r10_450bps_hac
  FLO-MIN110 SQK-RLI001           dna_r10_450bps_hac
  FLO-MIN110 VSK-VBK001           dna_r10_450bps_hac
  FLO-MIN110 VSK-VSK001           dna_r10_450bps_hac
  FLO-MIN110 VSK-VSK002           dna_r10_450bps_hac
  FLO-MIN110 SQK-16S024 included  dna_r10_450bps_hac
  FLO-MIN110 SQK-PCB109 included  dna_r10_450bps_hac
  FLO-MIN110 SQK-RBK001 included  dna_r10_450bps_hac
  FLO-MIN110 SQK-RBK004 included  dna_r10_450bps_hac
  FLO-MIN110 SQK-RLB001 included  dna_r10_450bps_hac
  FLO-MIN110 SQK-LWB001 included  dna_r10_450bps_hac
  FLO-MIN110 SQK-PBK004 included  dna_r10_450bps_hac
  FLO-MIN110 SQK-RAB201 included  dna_r10_450bps_hac
  FLO-MIN110 SQK-RAB204 included  dna_r10_450bps_hac
  FLO-MIN110 SQK-RPB004 included  dna_r10_450bps_hac
  FLO-MIN110 VSK-VMK001 included  dna_r10_450bps_hac
  FLO-MIN110 VSK-VMK002 included  dna_r10_450bps_hac
  FLO-PRO001 SQK-LSK109           dna_r9.4.1_450bps_hac_prom
  FLO-PRO001 SQK-LSK109-XL          dna_r9.4.1_450bps_hac_prom
  FLO-PRO001 SQK-DCS109           dna_r9.4.1_450bps_hac_prom
  FLO-PRO001 SQK-PCS109           dna_r9.4.1_450bps_hac_prom
  FLO-PRO001 SQK-PRC109           dna_r9.4.1_450bps_hac_prom
  FLO-PRO001 SQK-PCB109 included  dna_r9.4.1_450bps_hac_prom
  FLO-PRO002 SQK-LSK109           dna_r9.4.1_450bps_hac_prom
  FLO-PRO002 SQK-LSK109-XL          dna_r9.4.1_450bps_hac_prom
  FLO-PRO002 SQK-DCS109           dna_r9.4.1_450bps_hac_prom
  FLO-PRO002 SQK-PCS109           dna_r9.4.1_450bps_hac_prom
  FLO-PRO002 SQK-PRC109           dna_r9.4.1_450bps_hac_prom
  FLO-PRO002 SQK-PCB109 included  dna_r9.4.1_450bps_hac_prom
  FLO-MIN111 SQK-CAS109           dna_r10.3_450bps_hac
  FLO-MIN111 SQK-CS9109           dna_r10.3_450bps_hac
  FLO-MIN111 SQK-DCS108           dna_r10.3_450bps_hac
  FLO-MIN111 SQK-DCS109           dna_r10.3_450bps_hac
  FLO-MIN111 SQK-LRK001           dna_r10.3_450bps_hac
  FLO-MIN111 SQK-LSK108           dna_r10.3_450bps_hac
  FLO-MIN111 SQK-LSK109           dna_r10.3_450bps_hac
  FLO-MIN111 SQK-LSK109-XL          dna_r10.3_450bps_hac
  FLO-MIN111 SQK-LSK110           dna_r10.3_450bps_hac
  FLO-MIN111 SQK-LWP001           dna_r10.3_450bps_hac
  FLO-MIN111 SQK-PCS108           dna_r10.3_450bps_hac
  FLO-MIN111 SQK-PCS109           dna_r10.3_450bps_hac
  FLO-MIN111 SQK-PRC109           dna_r10.3_450bps_hac
  FLO-MIN111 SQK-PSK004           dna_r10.3_450bps_hac
  FLO-MIN111 SQK-RAD002           dna_r10.3_450bps_hac
  FLO-MIN111 SQK-RAD003           dna_r10.3_450bps_hac
  FLO-MIN111 SQK-RAD004           dna_r10.3_450bps_hac
  FLO-MIN111 SQK-RAS201           dna_r10.3_450bps_hac
  FLO-MIN111 SQK-RLI001           dna_r10.3_450bps_hac
  FLO-MIN111 VSK-VBK001           dna_r10.3_450bps_hac
  FLO-MIN111 VSK-VSK001           dna_r10.3_450bps_hac
  FLO-MIN111 VSK-VSK002           dna_r10.3_450bps_hac
  FLO-MIN111 SQK-16S024 included  dna_r10.3_450bps_hac
  FLO-MIN111 SQK-PCB109 included  dna_r10.3_450bps_hac
  FLO-MIN111 SQK-RBK001 included  dna_r10.3_450bps_hac
  FLO-MIN111 SQK-RBK004 included  dna_r10.3_450bps_hac
  FLO-MIN111 SQK-RLB001 included  dna_r10.3_450bps_hac
  FLO-MIN111 SQK-LWB001 included  dna_r10.3_450bps_hac
  FLO-MIN111 SQK-PBK004 included  dna_r10.3_450bps_hac
  FLO-MIN111 SQK-RAB201 included  dna_r10.3_450bps_hac
  FLO-MIN111 SQK-RAB204 included  dna_r10.3_450bps_hac
  FLO-MIN111 SQK-RPB004 included  dna_r10.3_450bps_hac
  FLO-MIN111 VSK-VMK001 included  dna_r10.3_450bps_hac
  FLO-MIN111 VSK-VMK002 included  dna_r10.3_450bps_hac
  FLO-FLG001 SQK-CAS109           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-CS9109           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-DCS108           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-DCS109           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-LRK001           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-LSK108           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-LSK109           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-LSK109-XL          dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-LSK110           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-LWP001           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-PCS108           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-PCS109           dna_r9.4.1_450bps_hac
  FLO-FLG001 SQK-PRC109           dna_r9.4.1_450bps_hac
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
  FLO-MIN106 SQK-CS9109           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-DCS108           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-DCS109           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-LRK001           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-LSK108           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-LSK109           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-LSK109-XL          dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-LSK110           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-LWP001           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-PCS108           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-PCS109           dna_r9.4.1_450bps_hac
  FLO-MIN106 SQK-PRC109           dna_r9.4.1_450bps_hac
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
  FLO-FLG001 SQK-RNA001           rna_r9.4.1_70bps_hac
  FLO-FLG001 SQK-RNA002           rna_r9.4.1_70bps_hac
  FLO-MIN106 SQK-RNA001           rna_r9.4.1_70bps_hac
  FLO-MIN106 SQK-RNA002           rna_r9.4.1_70bps_hac
  FLO-MIN107 SQK-RNA001           rna_r9.4.1_70bps_hac
  FLO-MIN107 SQK-RNA002           rna_r9.4.1_70bps_hac
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
  FLO-PRO111 SQK-LSK109           dna_r10.3_450bps_hac_prom
  FLO-PRO111 SQK-LSK109-XL          dna_r10.3_450bps_hac_prom
  FLO-PRO111 SQK-LSK110           dna_r10.3_450bps_hac_prom
  FLO-PRO111 SQK-DCS109           dna_r10.3_450bps_hac_prom
  FLO-PRO111 SQK-PCS109           dna_r10.3_450bps_hac_prom
  FLO-PRO111 SQK-PRC109           dna_r10.3_450bps_hac_prom
  FLO-PRO111 SQK-PCB109 included  dna_r10.3_450bps_hac_prom
  


Our dataset was generated using the FLO-MIN106 flowcell, and the LSK109 kit, pick the appropriate model.

**Note:** You need to add ".cfg" to the model name in the command line or the basecaller exits with an error because the .cfg could not be found.

Try to get the basecalling running, use the following optional arguments::

  --compress_fastq                  Compress fastq output files with gzip.
  --num_callers arg                 Number of parallel basecallers to create.
  --cpu_threads_per_caller arg      Number of CPU worker threads per basecaller.

**Important:** You have 14 CPUs in your virtual machine. The number of callers multiplied with the number of CPU threads per caller should not exceed 14. 

If you are stuck, you can skip to the next page and get help with the command.

References
^^^^^^^^^^

**guppy** https://nanoporetech.com/

