## Experiments and Performance Results

Click <a href="http://sophieyanzhao.github.io">here</a> to go back to Homepage.

## Table of Contents
1. [Data Preprocessing](#i-data-preprocessing)
  * [Speedup plot MapReduce](#speedup-plot-mapreduce)
  * [Efficiency plot](#efficiency-plot)
2. [Running SGD with RNN for Sentiment Analysis](#ii-rnn-sgd)
  * [Code Baseline](#code-baseline)

### I. Data Preprocessing

The main overhead comes from sequential code. First, to remove stopwords, we need to compare each word in the sequence with intended stopwords to determine whether we remove this word from the sequence. We save the stopwords in a set instead of a list to shorten the runtime from O(N) to O(1). Moreover, when we combine the h5 files, it takes a long time to write data to h5 files, especially when we save as many different datasets within the h5 file. Ideally, we would want to save all data in one single h5 dataset. However, we need to save data in a single array before saving it as a dataset, and we are restricted by the memory size of the instance. Therefore, we decide to save data into eight separate datasets with equal size. For faster reading during the training process, the datasets in the h5 file are chunked, so that when the data loader accesses a single entry, it does not need to load any other data into the memory.

A second source of overhead is communication. To avoid overwriting, each node in the EMR cluster writes its own h5 file and uploads to the S3 bucket. This time is reduced by having multiple nodes process the data and each upload a section of processed data at the same time.

The speedup of using different numbers of worker nodes in the cluster is illustrated below. We observe that the speedup is nonlinear, and the efficiency is decreasing, indicating weak scaling.

***Speedup plot of running MapReduce on different numbers of worker nodes***

***Efficiency plot (SPEEDUP/#Nodes)***




### II. Running SGD with RNN for Sentiment Analysis
#### Code Baseline

We ran sequential RNN(1 node and 1 GPU with AWS g3.4xlarge Instance). Results are shown below:

|epoch|time(s)|test acc| test f1|
|-----|-------|--------|--------|
|1    |3840   |81.41%  |0.85    |
|2    |3542   |83.48%  |0.86    |
|3    |3572   |83.59%  |0.86    |
|4    |3570   |83.73%  |0.86    |
|5    |3605   |84.36%  |0.86    |
|6    |3534   |84.29%  |0.86    |
|7    |3564   |84.22%  |0.86    |
|8    |3564   |84.76%  |0.86    |
|9    |3544   |84.43%  |0.86    |
|10   |3573   |84.96%  |0.86    |

*Total training time: 9.98 hours*

#### Strong Scaling 
