Get the remaining results 
=======================

If you feel like you want to repeat the previous steps for the remaining datasets and  if you are good in time, feel free to do so. 

Please rename the result file you have generated to::

  ~/workdir/results_artic/barcode_01.fasta

with::

  mv ~/workdir/results_artic/barcode_<number>_pilon.fasta  ~/workdir/results_artic/barcode_<number>.fasta
  
Then download the remaining results (pick the commands accordingly, you don't need barcode_01.fasta, if you succeeded in the previous steps)::
  
  cd ~/workdir/results_artic/
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/barcode_01.fasta
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/barcode_02.fasta
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/barcode_03.fasta
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/barcode_04.fasta
  wget https://openstack.cebitec.uni-bielefeld.de:8080/swift/v1/coursedata2020/barcode_05.fasta

Now we can start to use nextstrain.
