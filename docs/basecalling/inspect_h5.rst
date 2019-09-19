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

This will output something like this::
  GROUP "/" {
   ATTRIBUTE "file_version" {
      DATATYPE  H5T_STRING {
         STRSIZE H5T_VARIABLE;
         STRPAD H5T_STR_NULLTERM;
         CSET H5T_CSET_UTF8;
         CTYPE H5T_C_S1;
      }
      DATASPACE  SCALAR
      DATA {
      (0): "2.0"
      }
   }
   GROUP "read_0061d165-af04-4c39-ad5e-8c4ebe3caa80" {
      ATTRIBUTE "run_id" {
         DATATYPE  H5T_STRING {
            STRSIZE 40;
            STRPAD H5T_STR_NULLPAD;
            CSET H5T_CSET_ASCII;
            CTYPE H5T_C_S1;
         }
         DATASPACE  SCALAR
         DATA {
         (0): "b110eefd3ba5e91817c69585fcd2257218eeb796"
         }
      }
      GROUP "PreviousReadInfo" {
  [...]



 
References
^^^^^^^^^^

**HDF5 tools** https://support.hdfgroup.org/products/hdf5_tools/

