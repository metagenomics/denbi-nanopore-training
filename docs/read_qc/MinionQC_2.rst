MinIONQC(2)
--------

We need to run Rscript with MinIONQC.R::

  MinIONQC.R

... and specify the following options:

+------------------------------------------+-------------------------+---------------------------------------------+
| What?                                    | parameter               | Our value                                   |
+==========================================+=========================+=============================================+
| The input directory                      | -i                      | ~/workdir/data_artic/basecall_tiny_<number>/ |
+------------------------------------------+-------------------------+---------------------------------------------+ 
| The output directory                     | -o                      | ~/workdir/data_artic/MinIONQC/              |
+------------------------------------------+-------------------------+---------------------------------------------+
| The number of processors to be used      | -p                      | 14                                          |
+------------------------------------------+-------------------------+---------------------------------------------+


The complete command is::
  
  cd ~/workdir/data_artic/
  MinIONQC.R -i basecall_tiny_<number> -o MinIONQC -p 14
    
This will create several analysis plots. After that, you can load the plots in your web browser by using a file browser. You could use::

  firefox ~/workdir/data_artic/MinIONQC/basecall_tiny_<number>/*.png
  
to load all images at once in firefox.
  
  
Again, check out the corresponding home page to learn more about all generated results: https://github.com/roblanf/minion_qc
