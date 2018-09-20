The Tutorial Data Set
================================

We are going to download the tutorial dataset and some precomputed results into the virtual machine. First, create a workdir in your home directory::

  mkdir ~/workdir

The test dataset is already located in your virtual machine. Move it into your working directory::

  cd workdir
  mv ~/Results.tar.gz ~/workdir/Results.tar.gz
  mv ~/Data.tar.gz ~/workdir/Data.tar.gz

Then, unpack both tar archives::

  tar -xzvf Data.tar.gz
  tar -xzvf Results.tar.gz

and remove the tar archives::

  rm Data.tar.gz
  rm Results.tar.gz
  
 
