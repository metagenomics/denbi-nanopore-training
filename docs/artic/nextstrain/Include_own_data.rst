Include own data
----------------

As we have already seen, the data files are located in::

  ~/workdir/ncov/data/
  
All you need to do, to include your own data, is append the consensus fasta sequences to the fasta file, and add metadata to the metadata file.

Add fasta sequences
^^^^^^^^^^^^^^^^^^^

Copy the consensus sequences to the data folder::

  cat ~/workdir/results_artic/barcode*consensus.fasta > ~/workdir/ncov/data/consensus_sequences.fasta
  
Then open the sequence file in an editor::
  
  gedit ~/workdir/ncov/data/consensus_sequences.fasta
  
and give short names to the sequences like

  Germany/NRW-B0001/2020
  Germany/NRW-B0002/2020
  Germany/NRW-B0003/2020
  Germany/NRW-B0004/2020
  Germany/NRW-B0005/2020

Then append the fasta file to the example sequences with::
  
  cat  ~/workdir/ncov/data/consensus_sequences.fasta >> ~/workdir/ncov/data/example_sequences.fasta
  
  
Add metadata
^^^^^^^^^^^^^^^^^^^
  
Open the metadata file with::

  gedit ~/workdir/ncov/data/example_metadata.tsv
  
And add metadata lines for our data.


Rerun analysis
^^^^^^^^^^^^^^

Then make sure, the nextstrain environment is active::

  conda activate nextstrain\

Run the basic nextstrain worflow with::

  cd ~/workdir/ncov/
  snakemake --cores 14 --profile ./my_profiles/getting_started

Then start the auspice server::

  cd ~/workdir/ncov
  nextstrain view auspice/
  
The output should tell you, where to find the results. You can start   

And view the results in a browser with::

  firefox http://127.0.0.1:4000


That's it for the short nextstrain tutorial, you can check out the official nextstrain site for many more things you can do with nextstrain, which is way beyond the scope of this workshop.


References
^^^^^^^^^^

**Nextstrain SARS Cov2 Tutorial** https://nextstrain.github.io/ncov/ 
