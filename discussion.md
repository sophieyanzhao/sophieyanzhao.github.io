# Discussion
Click <a href="http://sophieyanzhao.github.io">here</a> to go back to Homepage.

## Table of Contents
1. [Goals Achieved](#goals-achieved)
  * [Machine Learning](#machine-learning)
  * [Data Processing](#data-processing)
  * [Distributed Computing](#distributed-computing)
2. [Lessons Learnt and Insights](#lessons-learnt-and-insights)
3. [Future Work](#future-work)



## Goals Achieved:
### Machine Learning 
The null classifier predicting majority class only achieves 78% accuracy. This is our baseline model.  After experimenting with different loss functions, neural network structures and class weights, we have achieved a best test accuracy around 88% and an f1 score of 87% (we give equal weights to both classes). High f1 score indicates that recall and precision are quite balanced despite the challenge of having only 22% data coming from the negative class.  

### Data Processing
The dataset contains around 142 millions reviews, with maximum sentence length of over 2000 words. All data preprocessing tasks are carried out on MapReduce and from then onwards reviews are of fixed-length number format. We experimented with multiple storage formats and finally managed large files with hdf5.

### Distributed Computing
We successfully deployed RNN model to multiple GPU’s and carried out experiments with varying number of nodes and batch sizes. We also implemented a **dynamic load balancer** that distributes batches of deferring sizes to GPUs based on their performance at the start of each epoch

## Lessons Learnt and Insights:
1.	Money and speed tradeoff: [insert a table]
2.	It is impossible to load big data files into memory via traditional means. It is necessary to utilize format like HDF5 that allows assignment of dataset without loading it into memory.
3.	By profiling GPU, we found that the lowest one is a serious bottleneck, which motivates our dynamic load balancer.
4.	Installing packages on EMR worker nodes can be achieved by adding custom bootstrap actions.

## Future Work:
1.	Currently, files are shared via a Network File System (NFS). Keeping a local copy can help mitigate resources contention problem when too many nodes are accessing the same NFS.
2.	Load balancer adjust batch size on each GPU every epoch. Adaptively adjust batch size after a smaller time interval will help speed up the application significantly. 
3.	It will be interesting to test our application on the newly released P3 GPU instances.

## References

“PyTorch 1.0 Distributed Trainer with Amazon AWS¶.” PyTorch 1.0 Distributed Trainer with Amazon AWS - PyTorch Tutorials 1.1.0.dev20190507 Documentation, pytorch.org/tutorials/beginner/aws_distributed_training_tutorial.html.

Subramanian, Vishnu, and Manas Agarwal. Deep Learning with PyTorch: a Practical Approach to Building Neural Network Models Using PyTorch. Packt Publishing, 2018.
