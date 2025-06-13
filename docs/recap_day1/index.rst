Recap Day 1
===========

If you got lost on day 1, no problem, we will continue with larger read files, that contain approximately 10x the reads you used yesterday, which should be around 600x coverage. Make sure, that your data directory::

  ~/workdir/data_artic/
  
exists. If not, create it::

  mkdir ~/workdir/data_artic/

All you need to go on is the basecalled and porechopped data (choose 1). You can get the data with::

  cd ~/workdir/data_artic/
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/basecall_small_porechopped_01.fastq.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/basecall_small_porechopped_02.fastq.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/basecall_small_porechopped_03.fastq.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/basecall_small_porechopped_04.fastq.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/basecall_small_porechopped_05.fastq.gz

Then rename your choice with::

  mv ~/workdir/data_artic/basecall_small_porechopped_<number>.fastq.gz ~/workdir/data_artic/basecall_small_porechopped.fastq.gz

When you are done, we can continue with the tutorial for day 2.
