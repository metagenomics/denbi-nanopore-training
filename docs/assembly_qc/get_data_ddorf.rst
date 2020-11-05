Get the Duesseldorf dataset
===========================

The tutorial dataset is located in our object store. Download it with wget::::

  cd ~/workdir
  mkdir data
  cd data
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/Cov2_Ddorf_artic.fastq.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/Cov2_Ddorf_random.fastq.gz

There are two files, one run with random primers (Cov2_Ddorf_random.fastq.gz) and one run using the ARTIC primers (Cov2_Ddorf_artic.fastq.gz).
