Connect to your virtual machine 
================================

The first thing you need to do is to connect to your virtual machine with the X2Go Client. If you are working with your laptop and haven't installed it yet - you can get it here:
https://wiki.x2go.org/doku.php/download:start


Enter the IP of your virtual machine, the port (ssh port), the username "ubuntu" and select your ssh key. When you have successfully connected to your machine, open a terminal.

You can get all the information in the cloud portal under Overview->Instances. Then go to your virtual machine and click "How to connect".
There is an ssh command like::

  ssh -p PORT_NUMBER -i /path/to/your/ssh/private/key ubuntu@IP

This is the IP and Port-Number you need to enter in the X2Go Interface.

Start a terminal. 

If you want to disable the annoying system beeps::

  xset -b


Create a working directory
--------------------------

When you have successfully connected to your VM, let's create a working directory for the course. There is an ephemeral drive in /mnt/ with enough storage::

  df -h /mnt/
  
  Filesystem      Size  Used Avail Use% Mounted on
  /dev/vdb        147G   61M  140G   1% /mnt

It is owned by root::
  
  ls -ld /mnt/
  
  drwxr-xr-x 3 root root 4096 Sep 20  2018 /mnt/
  
Change the owner to ubuntu, so you can write and read files in the directory::

  sudo chown ubuntu:ubuntu /mnt/
  
And finally, create a link in your home directory to the mounted volume::

  ln -s /mnt/ workdir

Enter the workdir::

  cd ~/workdir
  
