---
title: Large-Scale Distributed Sentiment Analysis with RNN
---
## Introduction

### Motivation and Project Statement

In the current era, social medias are so common that people are constantly expressing their feelings through text. There are tremendous business values underlying this information. **Therefore, we hope to perform sentiment analysis with Recurrent Neural Networks (RNN) in order to uncover whether a piece of text has positive or negative sentiment.** 


### The Need for Big Data and HPC

This is a big data and big compute combined problem. It involves big data because in our selected dataset, we handle 92.45 GB of 142.8 million reviews. It involves big compute because during the traning process of RNN, we need to frequently calculate the loss and update the parameters. Moreover, because of the nature of natural language processing, the vector representations of the text has very high dimensionality.

For example, It took XXX minutes for data preprocessing on **HOW MANY NODES AND CPUS AND CPU MODEL** and 18 hours for the RNN model to run 10 epochs and achieve 80% test accuracy on a single instance of AWS p2.xlarge (Tesla K80).

### Our Solution

**DESCRIBE MAP REDUCE** We employ MapReduce on AWS cluster to first preprocess the large amount of data. HDF5 file format has been used to load the data without blowing up the memory. After preprocessing the data, we use a AWS GPU cluster to distribute the workload across multiple GPUs to speed up this process. 

### Comparison with Existing Work



### Table of Contents

- [Model](http://sophieyanzhao.github.io/model)
- [Reproduction Instruction](http://sophieyanzhao.github.io/reproduction)
- [Experiments and Performance Results](http://sophieyanzhao.github.io/performance)
- [Discussions](http://sophieyanzhao.github.io/discussion)
