Get the WGS dataset
===================

The tutorial dataset is located in our object store. Download it with wget::

  cd ~/workdir
  mkdir data
  cd data
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/Cov2_HK_WGS.fastq.gz

Since the original dataset is quite large, we reduce it to 10k reads, using seqtk, a nice tool for several operations on fasta and fastq files::

  cd ~/workdir/data
  seqtk sample Cov2_HK_WGS.fastq.gz 10000 | gzip > Cov2_HK_WGS_small.fastq.gz
  

Have a short look, on what is contained within the data directory::


References
^^^^^^^^^^

**seqtk** https://github.com/lh3/seqtk
