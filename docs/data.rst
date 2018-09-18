The Tutorial Data Set
================================

We are going to download the tutorial dataset and some precomputed results into the virtual machine. First, create a workdir in your home directory::

  mkdir ~/workdir

The test dataset is located in the object store of our cloud. Since it is publicly available, we can download it into our working directory using wget::

  cd workdir
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/nanopore_training_data/Results.tar.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/nanopore_training_data/Data.tar.gz

Then, unpack both tar archives::

  tar -xzvf Data.tar.gz
  tar -xzvf Results.tar.gz

and remove the tar archives::

  rm Data.tar.gz
  rm Results.tar.gz
  
 
