---
title: Large-Scale Distributed Sentiment Analysis with RNN
---
## Introduction

### Motivation and Project Statement

In the current era, social medias are so common that people are constantly expressing their feelings through text. There are tremendous business values underlying this information. **Therefore, we hope to perform sentiment analysis with Recurrent Neural Networks (RNN) in order to uncover whether a piece of text has positive or negative sentiment.** 

***SOLUTION***

### The Need for Big Data and HPC

This is a big data and big compute combined problem. It involves big data because in our selected dataset, we handle 92.45 GB of 142.8 million reviews. It involves big compute because during the traning process of RNN, we need to frequently perform Stochastic Gradient Descent (SGD). Moreover, because of the nature of natural language processing, the vector representations of the text can potentially have very high dimensionality.

For example, we tested our sequential code with 1% of the data locally. ***Description of instance...*** It took XXX minutes for data preprocessing and XXX minutes for the RNN model to run XX epochs and achieve XXX accuracy.


### Comparison with Existing Work

### Table of Contents

- [Model](http://sophieyanzhao.github.io/model)
- [Reproduction Instruction](http://sophieyanzhao.github.io/reproduction)
- [Experiments and Performance Results](http://sophieyanzhao.github.io/performance)
- [Discussions](http://sophieyanzhao.github.io/discussion)
