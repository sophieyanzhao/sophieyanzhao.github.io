# Discussion
## Goals Achieved:
1.	The null classifier predicting majority class only achieves 78% accuracy. This is our baseline model.  After experimenting with different loss functions, neural network structures and class weights, we have achieved a best test accuracy around 88% and an f1 score of 87% (we give equal weights to both classes). High f1 score indicates that recall and precision are quite balanced despite the challenge of having only 22% data coming from the negative class.  
2.	The dataset contains around 142 millions reviews, with maximum sentence length of over 2000 words. All data preprocessing tasks are carried out on MapReduce and from then onwards reviews are of fixed-length number format. We experimented with multiple storage formats and finally managed large files with hdf5.
3.	We successfully deployed RNN model to multiple GPU’s and carried out experiments with varying number of nodes and batch sizes. We also implemented a dynamic load balancer that distributes batches of deferring sizes to GPUs based on their performance at the start of each epoch

## Lessons learnt:
1.	Money and speed tradeoff: [insert a table]
2.	It is impossible to load big data files into memory via traditional means. It is necessary to utilize format like HDF5 that allows assignment of dataset without loading it into memory.
3.	By profiling GPU, we found that the lowest one is a serious bottleneck, which motivates our dynamic load balancer.
4.	Installing packages on EMR worker nodes can be achieved by adding custom bootstrap actions.

## Future work:
1.	Test Network File System and keeping local copy, resources contention
2.	Load balancer change batch size for a smaller time interval
3.	Test P3 instances (latest release)

## Interesting Insights
