## Experiments and Performance Results

Click <a href="http://sophieyanzhao.github.io">here</a> to go back to Homepage.

## Table of Contents
1. [Metrics of Performance](#i-metrics-of-performance)
2. [Data Preprocessing](#ii-data-preprocessing)
  * [Speedup plot MapReduce](#speedup-plot-mapreduce)
  * [Efficiency plot](#efficiency-plot)
3. [Running SGD with RNN for Sentiment Analysis](#iii-rnn-sgd)
  * [Code Baseline](#code-baseline)
  * [Experiment with different number of GPUs](#experiment-with-different-number-of-GPUs)
  

### Metrics of Performance

While measuring the performance in terms of parallelization, we focus on the following metric:

  * Throughput:
  
  Throughput is defined as the ratio of problem size to the running time. 
  
  * Speedup:
  
  In general, speedup is a number that measures the relative performance of two systems processing the same problem. Specifically, if the amount of time to complete a work unit with 1 processing element is t1, and the amount of time to complete the same unit of work with N processing elements is tN, the strong scaling speedup is t1/tn. Also, under the weal scaling setting, t1 and tn are both measured with different total problem size (same size for every node), i.e., t1 will change with the number of processor, Then the weak scaling speedup is calculated correspondingly. Note the definition of weak scaling speedup is usually obscure, here we use this definition.
  
  * Efficiency:
  
  Efficiency refers to the ratio of the speedup to the number of processors, which is ideally constant. 

### I. Data Preprocessing

The main overhead comes from sequential code. First, to remove stopwords, we need to compare each word in the sequence with intended stopwords to determine whether we remove this word from the sequence. We save the stopwords in a set instead of a list to shorten the runtime from O(N) to O(1). Moreover, when we combine the h5 files, it takes a long time to write data to h5 files, especially when we save as many different datasets within the h5 file. Ideally, we would want to save all data in one single h5 dataset. However, we need to save data in a single array before saving it as a dataset, and we are restricted by the memory size of the instance. Therefore, we decide to save data into eight separate datasets with equal size. For faster reading during the training process, the datasets in the h5 file are chunked, so that when the data loader accesses a single entry, it does not need to load any other data into the memory.

A second source of overhead is communication. To avoid overwriting, each node in the EMR cluster writes its own h5 file and uploads to the S3 bucket. This time is reduced by having multiple nodes process the data and each upload a section of processed data at the same time.

The speedup of using different numbers of worker nodes in the cluster is illustrated below. We observe that the speedup is nonlinear, and the efficiency is decreasing, indicating weak scaling.

***Speedup plot of running MapReduce on different numbers of worker nodes***

***Efficiency plot (SPEEDUP/#Nodes)***




### II. Running SGD with RNN for Sentiment Analysis
#### Code Baseline

We run sequential RNN (1 node with 1 GPU - AWS g3.4xlarge Instance), which would be our code baseline. Results are shown below:

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

#### Experiment with different number of GPUs

In order to see whether our model is *strongly scalable*, we run our model on the whole dataset with different number of GPUs. Specifically, we tried 1 GPU (1 g3.4xlarge instances), 2 GPUs (2 g3.4xlarge instances), 4 GPUs (4 g3.4xlarge instances) and 8 GPUs (2 g3.16xlarge instances). The results are shown in the following figure:

![p](strong_scaling.png)

Consistent with Facebook's paper, we can see the linear throughput and log-linear speed-up. However, the efficiency decreases as the number of GPUs increases, which indicates our model is not strongly scalable. 

Also, we investigate into the convergence of our RNN with different number of GPUs. In terms of the number of epochs and execution time, we set the loss value *0.13* as the convergence threshold, and obtain the results as follow:

![p](convergence_diff_gpus.png)

The results exactly match our intution: 

  * The principle of parallel SGD and its convergence is based on *Gradient Aggregation*. Basically, the aggregated gradient is used to approximate the true gradient, and when the number of epochs increases, this approxmation becoomes precise. Also, from the left plot, in terms of the number of epochs, we can see that 1-node version has highest rate of convergence, and 8-node version has lowes rate of convergence.

  * While the number of GPUs increasing, each GPU will handle a smaller part of data, which means the time of each epoch decreases. Also, the advantage of *Gradient Aggregation* is that the approxmation of gradient can be attained more quickly. From the right plot, in terms of the running time, the model with more GPUs converges faster, which sugeests the convergence is accelerated by data parallelism.
  
#### Experiment 






