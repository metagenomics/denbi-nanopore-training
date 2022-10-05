Get tutorial dataset
--------------------

We will download the tutorial dataset for SARS-Cov2 provided by nextstrain. First, enter your workdir::

  cd ~/workdir/
  
Then clone the repository::

  git clone https://github.com/nextstrain/ncov.git

The data is contained in the directory::

  ls -l ~/workdir/ncov/data/

  total 1724
  -rwxrwxr-x 1 ubuntu ubuntu  171914 Nov 19 09:26 example_metadata.tsv
  -rwxrwxr-x 1 ubuntu ubuntu 1589835 Nov 19 09:26 example_sequences.fasta.gz

There is a fasta file and a metadata file containing information about each sequence.

Extract the data fasta file, located in the repository::

  gunzip ~/workdir/ncov/data/example_sequences.fasta.gz
  
In the next step, we run the nextstrain basic workflow.

Inspect the data files with less or more::

  less ~/workdir/ncov/data/example_sequences.fasta
  
and::

  less ~/workdir/ncov/data/example_metadata.tsv
  
... to get an impression of what data needs to be provided for nextstrain.


References
^^^^^^^^^^

**Nextstrain SARS Cov2 Tutorial** https://nextstrain.github.io/ncov/ 
