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
  
We have 5 different barcodes available. The datasets are highly reduced in order to achieve reasonable runtimes. Download 3 of them (use 3 of the commands below)::

  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/barcode_tiny_01.tar.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/barcode_tiny_02.tar.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/barcode_tiny_03.tar.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/barcode_tiny_04.tar.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/barcode_tiny_05.tar.gz


Then, unpack the tar archives::

  tar -xzvf barcode0*.tar.gz

and remove the tar archives::

  rm barcode0*.tar.gz  

Have a short look, on what is contained within the barcode directory::

  ls -l ~/workdir/data_artic/barcode_tiny_<number>
  
*Note: From now on, we use the placeholder "<number>" for the number of the dataset, exchange that number accordingly in the example command lines.

There is only one fast5 file contained in the tiny dataset, usually you have a number of fast5 files like this::

  total 33703024
  -rw-rw-r-- 1 ubuntu ubuntu 85000469 Apr  9  2020 FAN22359_pass_barcode01_ca07d29b_0.fast5
  -rw-rw-r-- 1 ubuntu ubuntu 85265241 Apr  9  2020 FAN22359_pass_barcode01_ca07d29b_1.fast5
  -rw-rw-r-- 1 ubuntu ubuntu 85383688 Apr  9  2020 FAN22359_pass_barcode01_ca07d29b_10.fast5
  -rw-rw-r-- 1 ubuntu ubuntu 86318724 Apr  9  2020 FAN22359_pass_barcode01_ca07d29b_100.fast5
  -rw-rw-r-- 1 ubuntu ubuntu 86630435 Apr  9  2020 FAN22359_pass_barcode01_ca07d29b_101.fast5
  -rw-rw-r-- 1 ubuntu ubuntu 86527663 Apr  9  2020 FAN22359_pass_barcode01_ca07d29b_102.fast5
  -rw-rw-r-- 1 ubuntu ubuntu 86645812 Apr  9  2020 FAN22359_pass_barcode01_ca07d29b_103.fast5
