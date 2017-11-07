***************
FastQC Quality Control
***************

FastQC aims to provide a simple way to do some quality control checks
on raw sequence data coming from high throughput sequencing
pipelines. It provides a modular set of analyses which you can use to
give a quick impression of whether your data has any problems of which
you should be aware before doing any further analysis.

The main functions of FastQC are

* Import of data from BAM, SAM or FastQ files (any variant)
* Providing a quick overview to tell you in which areas there may be problems
* Summary graphs and tables to quickly assess your data
* Export of results to an HTML based permanent report
* Offline operation to allow automated generation of reports without running the interactive application

See the `FastQC home page <http://www.bioinformatics.babraham.ac.uk/projects/fastqc/>`_ for more info.

To run ``FastQC`` on our data, simply type::

  cd /vol/spool/workdir/assembly
  fastqc read1.fq read2.fq

After ``FastQC`` finished running, copy the results to your ``public_html`` directory::

  cp -r read?_fastqc* ~/public_html/

Now you can access the report at::

    http://<YOUR_OPENSTACK_INSTANCE_IP_ADDRESS>/~ubuntu/read1_fastqc.html
    http://<YOUR_OPENSTACK_INSTANCE_IP_ADDRESS>/~ubuntu/read2_fastqc.html

Check out the `FastQC home page <http://www.bioinformatics.babraham.ac.uk/projects/fastqc/>`_ for examples
of reports including bad data.

.. toctree::
   :maxdepth: 1

