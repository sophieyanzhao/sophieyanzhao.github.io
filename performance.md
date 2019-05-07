## Experiments and Performance Results

## Table of Contents
1. [Data Preprocessing](#i-data-preprocessing)
  * [Speedup plot MapReduce](#speedup-plot-mapreduce)
  * [Efficiency plot](#efficiency-plot)
2. [RNN+SGD](#ii-rnn-sgd)

### I. Data Preprocessing

The main overhead comes from sequential code. First, to remove stopwords, we need to compare each word in the sequence with intended stopwords to determine whether we remove this word from the sequence. We save the stopwords in a set instead of a list to shorten the runtime from O(N) to O(1). Moreover, when we combine the h5 files, it takes a long time to write data to h5 files, especially when we save as many different datasets within the h5 file. Ideally, we would want to save all data in one single h5 dataset. However, we need to save data in a single array before saving it as a dataset, and we are restricted by the memory size of the instance. Therefore, we decide to save data into eight separate datasets with equal size. For faster reading during the training process, the datasets in the h5 file are chunked, so that when the data loader accesses a single entry, it does not need to load any other data into the memory.

A second source of overhead is communication. To avoid overwriting, each node in the EMR cluster writes its own h5 file and uploads to the S3 bucket. This time is reduced by having multiple nodes process the data and each upload a section of processed data at the same time.

The speedup of using different numbers of worker nodes in the cluster is illustrated below. We observe that the speedup is nonlinear, and the efficiency is decreasing, indicating weak scaling.

***Speedup plot of running MapReduce on different numbers of worker nodes***

***Efficiency plot (SPEEDUP/#Nodes)***




### II. RNN + SGD
