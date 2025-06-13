ARTIC pipeline
=======================

In this part of the tutorial, we are using the ARTIC pipeline to generate a consensus sequence from the Wuhan reference and our amplicon data.

The complete ARTIC bioinformatics SOP can be found here:
https://artic.network/ncov-2019/ncov2019-bioinformatics-sop.html

Since we already have basecalled and demultiplexed data, we start with the step "Read filtering".

The ARTIC pipeline is installed in a conda environment which we need to activate with::

  conda activate artic-ncov2019
  
When activated, your shell looks like this::

  (artic-ncov2019) ubuntu@nanopore-course-2020:~$
  
This indicates, that the artic-ncov2019 environment is active. When no environment is active, your shell indicates that with "(base)" at the start of the line like this::

  (base) ubuntu@nanopore-course-2020:~$
  
This is for example the case, when you open a new shell. You can also deactivate the active conda environment with::

  conda deactivate

Always make sure the artic-ncov2019 environment is active in this part of the tutorial.

.. toctree::
   :maxdepth: 1

   Length_filtering_1
   Length_filtering_2
   Consensus_sequence_1
   Consensus_sequence_2
