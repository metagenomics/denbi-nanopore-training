The Tutorial Data Set
================================

The first thing you need to do is to connect to your virtual machine with the X2Go Client. If you are working with your laptop and haven't installed it yet - you can get it here:
https://wiki.x2go.org/doku.php/download:start

Enter the IP of your virtual machine, the port, the username "ubuntu" and select your ssh key. When you have successfully connected to your machine, open a terminal.

As you have started the VM with a volume attached, this volume needs to be given to the ubuntu user for easy access::

  sudo chown ubuntu:ubuntu /mnt/volume/
  
Create a link in your home directory to the mounted volume::

  ln -s /mnt/volume/ workdir 

The tutorial dataset is located in our object store. We have also prepared some precomputed results.You can get both here (Group 1)::

  cd ~/workdir
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/nanopore_course_data/Data_Group1.tar.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/nanopore_course_data/Results_Group1.tar.gz

... and for Group 2::

  cd ~/workdir
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/nanopore_course_data/Data_Group2.tar.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/nanopore_course_data/Results_Group2.tar.gz

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

If you want to disable system beep sounds::

  xset -b
