---
title: Large-Scale Distributed Sentiment Analysis with RNN
---
## Introduction

### Motivation and Project Statement

In the current era, social medias are so common that people are constantly expressing their feelings through text. There are tremendous business values underlying this information. **Therefore, we hope to perform sentiment analysis with Recurrent Neural Networks (RNN) in order to uncover whether a piece of text has positive or negative sentiment.** 


### The Need for Big Data and HPC

This is a big data and big compute combined problem. It involves big data because in our selected dataset, we handle 92.45 GB of 142.8 million reviews. It involves big compute because during the training process of RNN, we need to frequently calculate the loss and update the parameters. Moreover, because of the nature of natural language processing, the vector representations of the text has very high dimensionality.

For example, it is simply impossible to perform data preprocessing on any cluster with fewer than eight m4.xlarge nodes because the nodes get unstable, and it takes 18 hours for the RNN model to run 10 epochs and achieve 80% test accuracy on a single instance of AWS p2.xlarge (NVIDIA Tesla K80).

### Our Solution

We employ MapReduce on AWS cluster to first preprocess the large amount of data. The mapper processes the input line by line, and the reducer combines the sorted intermediate output. HDF5 file format has been used to load the data without blowing up the memory. After processing the data, we use an AWS GPU cluster to distribute the workload across multiple GPUs by using large minibatch technique to speed up the RNN training on Pytorch and using its MPI interface with NCCL backend for communication between nodes. 

### Comparison with Existing Work

This parallelization solution has been inspired by Goyal et al.'s "Accurate, Large Minibatch SGD: Training ImageNet in 1 hour" on how they used Caffe2 with 256 GPUs to train ResNet-50 with large minibatch in 1 hour from 8 GPUs taking 29 hours. This paper introduces a variety of empirical experience for training large-scale neural network efficiently. For example, they discuss the choice of the bacth size and learning rate (linear scaling), batch normalization, and communication issues in distributed SGD, such as gradient aggregation. In our case, our work is based on Pytorch and we parallelize a RNN instead, and we focus on parallelization tradeoffs, such as time versus convergence and cost versus speed. 

### Table of Contents

- [Model](http://sophieyanzhao.github.io/model)
- [Reproduction Instruction](http://sophieyanzhao.github.io/reproduction)
- [Experiments and Performance Results](http://sophieyanzhao.github.io/performance)
- [Discussions](http://sophieyanzhao.github.io/discussion)
