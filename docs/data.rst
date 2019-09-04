The Tutorial Data Set
================================

The first thing you need to do is to connect to your virtual machine with the X2Go Client. If you are working with your laptop and haven't installed it yet - you can get it here:
https://wiki.x2go.org/doku.php/download:start

Enter the IP of your virtual machine, the port, the username "ubuntu" and select your ssh key. When you have successfully connected to your machine, open a terminal.


Then, you need to format and mount the volume we have attached to our virtual machine::

  sudo mkfs.ext4 /dev/vdc
  sudo mkdir /mnt/data/
  sudo mount /dev/vdc /mnt/data/
  
Create a link in your home directory to the mounted volume::

  ln -s /mnt/data/ workdir 

The tutorial dataset is located in our object store, you can get the data here::

  cd ~/workdir
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/nanopore_training_data/Results.tar.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/nanopore_training_data/Data.tar.gz

Then, unpack both tar archives::

  tar -xzvf Data.tar.gz
  tar -xzvf Results.tar.gz

and remove the tar archives::

  rm Data.tar.gz
  rm Results.tar.gz
  
 
