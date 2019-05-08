# Discussion
Click <a href="http://sophieyanzhao.github.io">here</a> to go back to Homepage.

## Table of Contents
1. [Goals Achieved](#goals-achieved)
  * [Machine Learning](#machine-learning)
  * [Data Processing](#data-processing)
  * [Distributed Computing](#distributed-computing)
2. [Lessons Learnt and Insights](#lessons-learnt-and-insights)
3. [Future Work](#future-work)
4. [References](#references)


## Goals Achieved:
### Machine Learning 
The null classifier predicting majority class only achieves 78% accuracy. This is our baseline model.  After experimenting with different loss functions, neural network structures and class weights, we have achieved a best test accuracy around 88% and an f1 score of 87% (we give equal weights to both classes). High f1 score indicates that recall and precision are quite balanced despite the challenge of having only 22% data coming from the negative class.  

### Data Processing
The dataset contains around 142 millions reviews, with maximum sentence length of over 2000 words. All data preprocessing tasks are carried out on MapReduce and from then onwards reviews are of fixed-length number format. We experimented with multiple storage formats and finally managed large files with hdf5.

### Distributed Computing
We successfully deployed RNN model to multiple GPU’s and carried out experiments with varying number of nodes and batch sizes. Through parallelization, we reduced the runtime from 18 hours using a single p2.xlarge down to 2.5 hours using 2 g3.16xlarge. We also implemented a **dynamic load balancer** that distributes batches of deferring sizes to GPUs based on their performance at the start of each epoch

## Lessons Learnt and Insights:
1.	Based on money-speed tradeoff, we learned that for RNN with intensive data loading:
   * G3.4xlarge is the cheapest option out all due to no communication overhead.
   * G3.16xlarge is the cheapest option amongst the fastest options (< 5 hours / speedup > 2) with only intra node communications between GPUs
   * Therefore, single node with multi-GPUs tend to have best value for money given lower communication overhead
   * Same GPU model should be used for multi node setup to avoid the slowest GPU becoming a bottleneck for the entire training.
2. Installing packages on EMR worker nodes can be achieved by adding custom bootstrap actions.
3. Our dynamic load balancer is motivated by the case of mixed GPU cluster or unstable performance for multi GPUs.
4.	It is impossible to load big data files into memory via traditional means. It is necessary to utilize format like HDF5 that allows assignment of dataset without loading it into memory.


## Future Work:
1.	Currently, files are shared via a Network File System (NFS). Keeping a local copy can help mitigate resources contention problem when too many nodes are accessing the same NFS.
2.	Load balancer adjust batch size on each GPU every epoch. Adaptively adjust batch size after a smaller time interval will help speed up the application significantly. 
3.	It will be interesting to test our application on the newly released P3 GPU instances for more money speed tradeoffs.

## References

[1] R. He, J. McAuley. Modeling the visual evolution of fashion trends
with one-class collaborative filtering. WWW, 2016

[2] J. McAuley, C. Targett, J. Shi, A. van den Hengel. Image-based
recommendations on styles and substitutes. SIGIR, 2015

[3] “PyTorch 1.0 Distributed Trainer with Amazon AWS.” PyTorch 1.0 Distributed Trainer with Amazon AWS - PyTorch Tutorials 1.1.0.dev20190507 Documentation, pytorch.org/tutorials/beginner/aws_distributed_training_tutorial.html.

[4] Subramanian, Vishnu, and Manas Agarwal. Deep Learning with PyTorch: a Practical Approach to Building Neural Network Models Using PyTorch. Packt Publishing, 2018.

[5] Davis, Matt. SnakeViz, jiffyclub.github.io/snakeviz/.

[6] Goyal, Priya, et al. "Accurate, large minibatch sgd: Training imagenet in 1 hour." arXiv preprint arXiv:1706.02677 (2017).

[7] Zinkevich, Martin, et al. "Parallelized stochastic gradient descent." Advances in neural information processing systems. 2010.
