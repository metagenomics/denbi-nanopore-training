Recap Day 2 / Preparations Day 3 
=======================

We have performed an assembly on the ARTIC dataset and since it didn't work well we did another using a WGS dataset which we also polished using racon and medaka.

Today, we are working with the ARTIC datasets again. Please go to your data_artic directory, and download all datasets, untar and remove the tar files::

  cd ~/workdir/data_artic/

  for i in {1..5} 
  do 
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/basecall_small_0$i.tar.gz
  tar -xzvf basecall_small_0$i.tar.gz
  rm basecall_small_0$i.tar.gz
  done

We also have some Illumina data fitting to our samples::

    for i in {1..5} 
    do 
    wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/Illumina_0$i.tar.gz
    tar -xzvf Illumina_0$i.tar.gz
    rm Illumina_0$i.tar.gz
    done


