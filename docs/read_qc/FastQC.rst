FastQC
------

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

QA with FASTQC
``````````````
Evaluate with fastqc::
  
  mkdir ~/www/1D_fastqc
  mkdir ~/www/1D2_fastqc
  fastqc -o www/fastqc_1D/ 1D_basecall.fastq
  fastqc -o www/fastqc_1D2/ 1D2basecall.fastq
  
After that, you can load the reports in your web browser::

  http://YOUR_OPENSTACK_INSTANCE_IP/

Check out the `FastQC home page <http://www.bioinformatics.babraham.ac.uk/projects/fastqc/>`_ for examples
of reports including bad data.
