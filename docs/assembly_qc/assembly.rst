Assembly with canu
==================
Canu is a fork of the Celera Assembler, designed for high-noise single-molecule sequencing (such as the PacBio RS II/Sequel or Oxford Nanopore MinION).

Canu is a hierarchical assembly pipeline which runs in four steps:
- Detect overlaps in high-noise sequences using MHAP
- Generate corrected sequence consensus
- Trim corrected sequences
- Assemble trimmed corrected sequences

Get a usage message of canu on how to use the assembler::

  canu --help

  usage:   canu [-version] [-citation] \
                [-correct | -trim | -assemble | -trim-assemble] \
                [-s <assembly-specifications-file>] \
                 -p <assembly-prefix> \
                 -d <assembly-directory> \
                 genomeSize=<number>[g|m|k] \
                [other-options] \
                [-pacbio-raw |
                 -pacbio-corrected |
                 -nanopore-raw |
                 -nanopore-corrected] file1 file2 ...

  example: canu -d run1 -p godzilla genomeSize=1g -nanopore-raw reads/*.fasta.gz 


    To restrict canu to only a specific stage, use:
      -correct       - generate corrected reads
      -trim          - generate trimmed reads
      -assemble      - generate an assembly
      -trim-assemble - generate trimmed reads and then assemble them

    The assembly is computed in the -d <assembly-directory>, with output files named
    using the -p <assembly-prefix>.  This directory is created if needed.  It is not
    possible to run multiple assemblies in the same directory.

    The genome size should be your best guess of the haploid genome size of what is being
    assembled.  It is used primarily to estimate coverage in reads, NOT as the desired
    assembly size.  Fractional values are allowed: '4.7m' equals '4700k' equals '4700000'

    Some common options:
      useGrid=string
        - Run under grid control (true), locally (false), or set up for grid control
          but don't submit any jobs (remote)
      rawErrorRate=fraction-error
        - The allowed difference in an overlap between two raw uncorrected reads.  For lower
          quality reads, use a higher number.  The defaults are 0.300 for PacBio reads and
          0.500 for Nanopore reads.
      correctedErrorRate=fraction-error
        - The allowed difference in an overlap between two corrected reads.  Assemblies of
          low coverage or data with biological differences will benefit from a slight increase
          in this.  Defaults are 0.045 for PacBio reads and 0.144 for Nanopore reads.
      gridOptions=string
        - Pass string to the command used to submit jobs to the grid.  Can be used to set
          maximum run time limits.  Should NOT be used to set memory limits; Canu will do
          that for you.
      minReadLength=number
        - Ignore reads shorter than 'number' bases long.  Default: 1000.
      minOverlapLength=number
        - Ignore read-to-read overlaps shorter than 'number' bases long.  Default: 500.
    A full list of options can be printed with '-options'.  All options can be supplied in
    an optional sepc file with the -s option.

    Reads can be either FASTA or FASTQ format, uncompressed, or compressed with gz, bz2 or xz.
    Reads are specified by the technology they were generated with:
      -pacbio-raw         <files>
      -pacbio-corrected   <files>
      -nanopore-raw       <files>
      -nanopore-corrected <files>

We will run the assembly in two steps.

Generate corrected reads
------------------------

Command::

  canu -correct -d ~/canu_correct -p 'canuAssembly' 'genomeSize=3m' 'gnuPlotTested=true' 'useGrid=false' 'maxThreads=16' 'minThreads=16' -nanopore-raw D1_basecall/workspace/pass/


Genereate and assemble trimmed reads
-----------------------

Command::
  canu -trim-assemble -d ~/canu_assembly -p 'canuAssembly' 'genomeSize=3M' 'useGrid=false' 'minThreads=16' 'maxThreads=16' 'gnuPlotTested=true' -nanopore-corrected ~/canu_correct/canuAssembly.correctedReads.fasta.gz -nanopore-corrected ~/D1_2_basecall/workspace/fastq_runid_* 
  
Or two commands? (not tested) ::
  canu -trim -d ~/canu_trim 'canuAssembly' 'genomeSize=3M' 'useGrid=false' 'minThreads=16' 'maxThreads=16' 'gnuPlotTested=true' -nanopore-corrected ~/canu_correct/canuAssembly.correctedReads.fasta.gz -nanopore-corrected ~/D1_2_basecall/workspace/fastq_runid_* 
  canu -assemble -d ~/canu_assemble 'canuAssembly' 'genomeSize=3M' 'useGrid=false' 'minThreads=16' 'maxThreads=16' 'gnuPlotTested=true' -nanopore-corrected ~/canu_trim/canuAssembly.trimmedReads.fasta.gz -nanopore-corrected ~/D1_2_basecall/workspace/fastq_runid_* 
  
  
  
  
