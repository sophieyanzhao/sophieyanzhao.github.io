## Experiments and Performance Results

### I. Data Preprocessing

Main overheads: 
1. sequential - compare and remove stopwords
 - solution: save stopwords in a set
2. sequential - write n dsets to an h5 file -> time; write 1 dset -> memory
  - solution: write 8 dsets, but posts problem for pytorch data loader get_item, so truncate to 2650000 in each dset.

The speedup of using different numbers of worker nodes in the cluster is illustrated below. We observe

***Speedup plot of running MapReduce on different numbers of worker nodes***



For faster reading, the datasets in the h5 file are chunked, so that when the data loader accesses a single entry, it does not need to load any other data into the memory.

### II. RNN + SGD