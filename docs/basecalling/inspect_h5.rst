Inspect the fast5 files
-------------------------

The raw signals in Nanopore sequencing are stored in HDF5 format. HDF stands for "Hierarchical Data Format", and it is quite similar to json. Terms used by HDF include Groups, Attributes and Datasets. A Group can contain Groups or Datasets and may have Attributes. A Dataset contains an array of datapoints. The way HDF5 is stored allows it to access individual Groups within the dataset fast and efficient.

The HDF5 tools can be used to display contents of HDF5 files. We will use two of them to explore our data::
  
  h5dump -- Enables a user to examine the contents of an HDF5 file and dump those contents to an ASCII file.
  h5ls -- Lists specified features of HDF5 file contents. 
  
In order to get the complete content of a fast5 in readable form, you can use::

  h5dump data/fast5_tiny/XXX.fast5 | more
  or for group2:
  h5dump data/fast5_tiny/GXB01322_20181217_FAK35493_GA10000_sequencing_run_Run00014_MIN106_RBK004_46674_0.fast5 | more

Inspect the output. The file starts with a root group::
  
  GROUP "/" {
  [...]


    




 
References
^^^^^^^^^^

**HDF5 tools** https://support.hdfgroup.org/products/hdf5_tools/

