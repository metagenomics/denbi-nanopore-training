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

First, copy the data into the working directory of your OpenStack instance using `scp`::

  cd /vol/porecourse/
  scp -r -i $PATH_TO_YOUR_SECRET_SSH_KEY_FILE Nanopore_small ubuntu@$YOUR_OPENSTACK_INSTANCE_IP:~/workspace/.

From here on, make sure you are actually working on the OpenStack
cloud instance (logged in via ssh) and not on your local workstation::

  ssh -X -i $PATH_TO_YOUR_SECRET_SSH_KEY_FILE ubuntu@$YOUR_OPENSTACK_INSTANCE_IP

