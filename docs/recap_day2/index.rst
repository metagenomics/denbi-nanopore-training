Recap Day 2 / Preparations Day 3 
=======================

We have performed an assembly on the ARTIC dataset and since it didn't work well we did another using a WGS dataset which we also polished using racon and medaka.

Today, we are working with the ARTIC datasets again. Please go to your data_artic directory, cleanu and download all datasets::

  cd ~/workdir/data_artic/

  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/basecall_small_01.tar.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/basecall_small_02.tar.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/basecall_small_03.tar.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/basecall_small_04.tar.gz
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/basecall_small_05.tar.gz

Then untar everyhting::

  for i in {1..5} 
  do tar -xzvf basecall_small_0$i.tar.gz
  done

and remove tar files::

  rm basecall_small_0?.tar.gz
  
