Get the remaining results 
=======================

If you feel like you want to repeat the previous steps for the remaining datasets and  if you are good in time, feel free to do so. 

Please rename the result file you have generated to::

  ~/workdir/results_artic/barcode_<number>.fasta

with::

  mv ~/workdir/results_artic/barcode_<number>_pilon.fasta  ~/workdir/results_artic/barcode_<number>.fasta
  
Then download the remaining results (pick the commands accordingly)::
  
  cd ~/workdir/results_artic/
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/basecall_01.fasta
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/basecall_02.fasta
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/basecall_03.fasta
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/basecall_04.fasta
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/basecall_05.fasta

Now we can start to use nextstrain.
