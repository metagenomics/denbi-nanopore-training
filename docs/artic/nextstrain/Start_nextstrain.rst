Start nextstrain
----------------

As with the ARTIC pipeline, nextstrain is also installed in a conda environment. Activate it with::

  conda activate nextstrain
  
Make sure, the conda environment is active during the next steps.

Run the basic nextstrain worflow with::

  cd ~/workdir/ncov/
  snakemake --cores 14 --profile ./my_profiles/getting_started

Then start the auspice server::

  cd ~/workdir/ncov
  nextstrain view auspice/
  
And view the results in a browser at::

  
