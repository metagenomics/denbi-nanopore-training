The Tutorial Data Set
================================

We have prepared a small toy data set for this tutorial. The data is
located in the `/vol/porecourse/` directory, which has the following
content:

+-------------------+---------------------------------------------------------------------------+
| File              | Content                                                                   |
+===================+===========================================================================+
| Nanopore_small    | Directory containing a small portion of our test dataset.                 |
+-------------------+---------------------------------------------------------------------------+

The complete test dataset is already located in the ubuntu home directory of your virtual machine.

First, copy the data to your OpenStack instance using `scp`:

  cd /vol/porecourse/
  scp -r -i <your openstack private key> Nanopore_small ubuntu@<your VM's IP>:~/.

From here on, make sure you are actually working on the OpenStack
cloud instance (logged in via ssh) and not on your local workstation.

Create a working directory in your home directory and symbolic links
to the data files::

  mkdir -p /vol/spool/workdir/assembly
  cd /vol/spool/workdir/assembly
  ln -s ~/WGS-data/* .

