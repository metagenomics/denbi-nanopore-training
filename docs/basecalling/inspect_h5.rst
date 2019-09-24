Inspect the fast5 files
-------------------------

The raw signals in Nanopore sequencing are stored in HDF5 format. HDF stands for "Hierarchical Data Format", and it is quite similar to json. Terms used by HDF include Groups, Attributes and Datasets. A Group can contain Groups or Datasets and may have Attributes. A Dataset contains an array of datapoints. The way HDF5 is stored allows it to access individual Groups within the dataset fast and efficient.

The HDF5 tools can be used to display contents of HDF5 files. We will use two of them to explore our data::
  
  h5dump -- Enables a user to examine the contents of an HDF5 file and dump those contents to an ASCII file.
  h5ls -- Lists specified features of HDF5 file contents. 
  
In order to get the complete content of a fast5 in readable form, you can use::

  h5dump data/fast5_tiny/GXB01322_20181217_FAK35493_GA10000_sequencing_run_Run00014_MIN106_RBK004_46674_0.fast5 | more

Inspect the output. The file starts with a root group::
  
  GROUP "/" {
  [...]
  
followed by the first read::
  
  GROUP "read_0061d165-af04-4c39-ad5e-8c4ebe3caa80" {
  [...]
  
at some point, the actual data is stored as a dataset::

  DATASET "Signal" {
    DATATYPE  H5T_STD_I16LE
    DATASPACE  SIMPLE { ( 104805 ) / ( H5S_UNLIMITED ) }
    DATA {
    (0): 450, 414, 428, 435, 445, 439, 439, 416, 432, 437, 410, 403,
    (12): 429, 410, 415, 426, 424, 409, 415, 416, 422, 421, 422, 418,
    [...]
  
To get an overview on all reads, you could use the h5ls command::

  h5ls data/fast5_tiny/GXB01322_20181217_FAK35493_GA10000_sequencing_run_Run00014_MIN106_RBK004_46674_0.fast5
  
This will give you a list of all reads::

  read_0061d165-af04-4c39-ad5e-8c4ebe3caa80 Group
  read_00e09394-e199-4738-bf1a-2d2f97dff4a8 Group
  read_01204611-3592-419c-9785-83c0fafa4c4a Group
  read_0130da91-b3ad-42bd-847e-0d2ce314ee48 Group
  read_015af2b6-ef6c-4c86-b598-02325d42fc6d Group
  read_01a509fd-a58c-4257-8136-600c22b4d053 Group
  read_01ed0343-0286-42f1-8ea3-0904d66521b6 Group
  read_025147d4-e101-426c-8b80-38d752d41dc8 Group
  [...]

Which you can simply count to get the number of reads in your fast5 file::
  
  h5ls data/fast5_tiny/GXB01322_20181217_FAK35493_GA10000_sequencing_run_Run00014_MIN106_RBK004_46674_0.fast5 | wc -l 
    
In order to inspect what is stored for an individual read, you can specify that read, as if it were a directory using h5ls::

  h5ls data/fast5_tiny/GXB01322_20181217_FAK35493_GA10000_sequencing_run_Run00014_MIN106_RBK004_46674_0.fast5/read_a3d14887-0d45-4ef5-8a20-42af8257053d
  or (group2):
  h5ls data/fast5_tiny/GXB01322_20181217_FAK35493_GA10000_sequencing_run_Run00014_MIN106_RBK004_46674_0.fast5/read_0061d165-af04-4c39-ad5e-8c4ebe3caa80
  
Which gives you the groups ("subdirectories") for that Read::

  PreviousReadInfo         Group
  Raw                      Group
  channel_id               Group
  context_tags             Group
  tracking_id              Group
  
Let's assume, we are interested in the raw data of a specific read::

  h5ls data/fast5_tiny/GXB01322_20181217_FAK35493_GA10000_sequencing_run_Run00014_MIN106_RBK004_46674_0.fast5/read_a3d14887-0d45-4ef5-8a20-42af8257053d/Raw
  or (group2):
  h5ls data/fast5_tiny/GXB01322_20181217_FAK35493_GA10000_sequencing_run_Run00014_MIN106_RBK004_46674_0.fast5/read_0061d165-af04-4c39-ad5e-8c4ebe3caa80/Raw
  
The output is::

  Signal                   Dataset {104805/Inf}
  
So we have reached the actual raw data (indicated by "Dataset"). To view a dataset, h5ls has a '-d' option::

  h5ls -d data/fast5_tiny/GXB01322_20181217_FAK35493_GA10000_sequencing_run_Run00014_MIN106_RBK004_46674_0.fast5/read_a3d14887-0d45-4ef5-8a20-42af8257053d/Raw/Signal
  or (group2):
  h5ls -d data/fast5_tiny/GXB01322_20181217_FAK35493_GA10000_sequencing_run_Run00014_MIN106_RBK004_46674_0.fast5/read_0061d165-af04-4c39-ad5e-8c4ebe3caa80/Raw/Signal
  
Which will give you the raw signal of that specific read::

  Signal                   Dataset {104805/Inf}
    Data:
    (0) 450, 414, 428, 435, 445, 439, 439, 416, 432, 437, 410, 403, 429, 410, 415, 426, 424, 409, 415, 416, 422, 421, 422, 418, 425, 424, 414, 419,
    (28) 434, 429, 424, 412, 423, 412, 411, 411, 409, 423, 421, 413, 408, 429, 422, 432, 432, 408, 438, 408, 428, 416, 418, 429, 427, 423, 434, 432,
    (56) 426, 418, 436, 440, 418, 415, 423, 428, 416, 419, 425, 430, 425, 423, 408, 428, 419, 424, 426, 426, 419, 428, 436, 421, 418, 412, 426, 430,
    (84) 438, 439, 426, 415, 444, 418, 419, 428, 433, 432, 415, 419, 426, 439, 411, 410, 414, 417, 426, 433, 430, 430, 412, 418, 418, 410, 423, 424,
    (112) 426, 412, 422, 410, 415, 416, 427, 407, 429, 439, 420, 430, 426, 420, 426, 424, 419, 424, 420, 415, 429, 418, 418, 424, 425, 425, 419,
    (139) 424, 424, 420, 419, 431, 440, 429, 418, 421, 421, 427, 421, 423, 410, 423, 432, 436, 426, 417, 425, 436, 425, 423, 418, 426, 425, 424,
    (166) 419, 422, 411, 427, 423, 424, 424, 423, 420, 430, 424, 426, 434, 405, 420, 419, 427, 423, 423, 432, 421, 430, 418, 433, 430, 424, 427,
    (193) 425, 421, 421, 437, 433, 422, 430, 412, 426, 416, 427, 426, 417, 420, 427, 417, 426, 427, 422, 435, 429, 425, 428, 428, 395, 432, 424,


Now that you have an idea of how the raw data out of the machine looks like, we can start the basecalling.

 
References
^^^^^^^^^^

**HDF5 tools** https://support.hdfgroup.org/products/hdf5_tools/
