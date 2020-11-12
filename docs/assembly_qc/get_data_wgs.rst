Get and inspect the WGS dataset
===============================

Get the data
------------

The dataset we are going to use is a whole genome shotgun project from an outbreak in Hongkong.

The dataset is located in our object store. Download it with wget::

  cd ~/workdir
  mkdir data_wgs
  cd data_wgs
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/Cov2_HK_WGS.fastq.gz

Since the original dataset is quite large, we reduce it to 50k reads, using seqtk, a nice tool for several operations on fasta and fastq files::

  cd ~/workdir/data_wgs
  seqtk sample Cov2_HK_WGS.fastq.gz 50000 | gzip > Cov2_HK_WGS_small.fastq.gz


Map the data and inspect with GenomeView
----------------------------------------




References
^^^^^^^^^^

**seqtk** https://github.com/lh3/seqtk
