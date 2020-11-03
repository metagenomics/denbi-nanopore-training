The Tutorial Data Sets
================================


We have two tutorial datasets prepared for you. 
One is a SARS-Cov2 WGS dataset from a patient in Hongkong. We will use that sample for the assembly part of the tutorial. The dataset can also be found in the SRA:
https://www.ncbi.nlm.nih.gov/sra/SRX8154258

The other one is our own amplicon based dataset created with ARTIC primers from an outbreak in Guetersloh/Germany. Since we don't have the raw data for the SRA dataset, we will start with the basecalling for our dataset. 

The tutorial dataset is located in our object store. First create a data folder, and enter it::

  cd ~/workdir
  mkdir data_artic
  cd data_artic
  
We have 5 different barcodes available, download 3 of them (use 3 of the commands below)::

  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/barcode01.tar.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/barcode02.tar.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/barcode03.tar.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/barcode04.tar.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/barcode05.tar.gz

Then, unpack the tar archives::

  tar -xzvf barcode0*.tar.gz

and remove the tar archives::

  rm barcode0*.tar.gz  

Have a short look, on what is contained within the barcode directory::

  ls -l ~/workdir/data_artic/barcode01

