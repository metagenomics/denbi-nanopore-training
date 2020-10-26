# de.NBI - CeBiTec Nanopore Workshop 2020 - Best Practice and SARS-CoV-2 applications - Overview course structure

## Day 1
### Start virtual machines
Live demo together with participants
### Connect to virtual machines
### Linux Introduction
Readthedocs??

## Day 2: Basecalling and ReadQC on ARTIC daaset
### Connect to Vm and create workdir link
### Get data and inspect
- Download our ARTIC dataset (full dataset and smaller dataset)
- Download Wuhan Reference (+Annotation!)
- Exercise?: Inspect the raw data using h5tools

### Basecalling
- (optional): Start Rampart
- Exercise: Start the basecalling with the smaller dataset
- (optional): Watch progress in Rampart
- Inspect the results
- (optional): Rampart for full dataset
- Start the basecalling with complete ARTIC dataset (or download finished basecalling)
- Exercise: Demultiplex 

### Read QC ARTIC dataset
- (optional) Use MinIONQC on ARTIC dataset
- Exercise: Use FastQC on ARTIC dataset
- Exercise: Use Porechop on ARTIC dataset
- (optional) Exercise: AlignQC with ARTIC dataset 
- Exercise: Mapping of reads to Wuhan Reference
- Inspect with IGV/Genomeview

## Day 3
### Assembly of ARTIC dataset
- Exercise: start canu with ARTIC dataset
- Inspect results
- (optional) Exercise: Map contigs to reference and inspect in IGV/Genomeview
- Notice, that it didn't work well... -> use other dataset for assembly

### Read QC WGS dataset
- Download WGS dataset
- Create subsample (or provide it)
- Exercise: Use FastQC on WGS dataset
- Exercise: Use Porechop on WGS dataset
- Exercise: Map WGS reads to reference and inspect in IGV/Genomeview

### Assembly of WGS dataset
- Exercise: Assembly with canu
- Exercise: Assembly evaluation with quast
- Exercise: Quality control by mapping

### Polishing
- Exercise: Polish with racon/minimap2 and medaka
- Exercise: Assembly evaluation with quast
- Hint for pilon, when Illumina data is available
- (optional) Exercise: Polish with nanopolish and evaluate with quast

## Day 4
### ARTIC dataset
- Exercise: Read filtering
- Exercise: Run ARTIC pipeline (medaka and nanopolish?)
- Exercise: Run ARTIC pipeline for all barcodes
- Evaluation: quast? IGV? ...
- Load results in nextstrain / phylogenetic tree

### Bonus?
Provide old course data and Exercises?
