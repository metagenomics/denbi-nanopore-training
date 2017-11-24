Circularizing the genome with circlator
---------------------------------------

The assembly of DNA sequence data is undergoing a renaissance thanks to emerging technologies capable of producing reads tens of kilobases long. Assembling complete bacterial and small eukaryotic genomes is now possible, but the final step of circularizing sequences remains unsolved. Circlator is a tool to automate assembly circularization and produce accurate linear representations of circular sequences. Circlator is available at http://sanger-pathogens.github.io/circlator/.

We will use circlator::

  cd ~/workdir/
  mkdir Annotation
  circlator minimus2 ~/Results/Pilon_after_nanopolish/Pilon_round5.fasta ~/workdir/Annotation/circlator
  
  



References
^^^^^^^^^^

**circlator** http://sanger-pathogens.github.io/circlator/
