The Tutorial Data Sets
================================


We have two tutorial datasets prepared for you. 
One is a SARS-Cov2 WGS dataset from a patient in Hongkong. We will use that sample for the assembly part of the tutorial. The dataset can also be found in the SRA:
https://www.ncbi.nlm.nih.gov/sra/SRX8154258

The other one is our own amplicon based dataset created with ARTIC primers from an outbreak in Guetersloh/Germany. Since we don't have the raw data for the SRA dataset, we will start with the basecalling for our dataset. 


The tutorial dataset is located in our object store. First create a data folder, then download the dataset::

  cd ~/workdir
  mkdir data_artic
  cd data_artic
  TODO: Download data from object store 


TODO: modify rest down here
  
Then, unpack the tar archive::

  tar -xzvf Data_Group1.tar.gz
  tar -xzvf Results_Group1.tar.gz

  or
  
  tar -xzvf Data_Group2.tar.gz
  tar -xzvf Results_Group2.tar.gz

and remove the tar archives::

  rm Data_Group1.tar.gz
  rm Results_Group1.tar.gz
  or
  rm Data_Group2.tar.gz
  rm Results_Group2.tar.gz
  

Have a short look, on what is contained within the data directory::

  ls -l ~/workdir/data/
  -rw-r--r-- 1 ubuntu ubuntu 4372654 Aug 30 08:24 Reference.fna
  drwxr-xr-x 2 ubuntu ubuntu   24576 Aug 30 08:24 fast5
  drwxrwxr-x 2 ubuntu ubuntu    4096 Sep  5 07:23 fast5_small
  drwxrwxr-x 2 ubuntu ubuntu    4096 Sep 12 08:01 fast5_tiny
  drwxr-xr-x 2 ubuntu ubuntu    4096 Aug 30 08:36 illumina

There are three folders with Nanopore fast5 data, a Reference genome for later comparisons and some illumina data.
