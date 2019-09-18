The Tutorial Data Set
================================

The first thing you need to do is to connect to your virtual machine with the X2Go Client. If you are working with your laptop and haven't installed it yet - you can get it here:
https://wiki.x2go.org/doku.php/download:start

Enter the IP of your virtual machine, the port, the username "ubuntu" and select your ssh key. When you have successfully connected to your machine, open a terminal.

As you have started the VM with a volume attached, this volume needs to be given to the ubuntu user for easy access::

  sudo chown ubuntu:ubuntu /mnt/data/
  
Create a link in your home directory to the mounted volume::

  ln -s /mnt/data/ workdir 

The tutorial dataset is located in our object store, you can get the data here (Group 1)::

  cd ~/workdir
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/nanopore_course_data/Data_Group1.tar.gz

... and for Group 2::

  cd ~/workdir
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/nanopore_course_data/Data_Group2.tar.gz

Then, unpack the tar archive::

  tar -xzvf Data_Group1.tar.gz
  or
  tar -xzvf Data_Group2.tar.gz

and remove the tar archives::

  rm Data_Group1.tar.gz
  or
  rm Data_Group2.tar.gz
  
Rename Data folder::

  mv Data_Group1 data
  or
  mv Data_Group2 data
  
If you want to disable system beep sounds::

  xset -b
