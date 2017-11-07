The Tutorial Data Set
================================

We have prepared a small toy data set for this tutorial. The data is
located in the `/vol/porecourse` directory, which has the following
content:

+---------------+--------------------------------------------+
| File          | Content                                    |
+===============+============================================+
| dir1/         | Directory containing ...                   |
+---------------+--------------------------------------------+
| dir2/         | Directory containing ...                   |
+---------------+--------------------------------------------+
| reads.fast5   | Reads...                                   |
+---------------+--------------------------------------------+

First, copy the data to your OpenStack instance using `scp`:

  scp ......

From here on, make sure you are actually working on the OpenStack
cloud instance (logged in via ssh) and not on your local workstation.

Create a working directory in your home directory and symbolic links
to the data files::

  mkdir -p /vol/spool/workdir/assembly
  cd /vol/spool/workdir/assembly
  ln -s ~/WGS-data/* .

