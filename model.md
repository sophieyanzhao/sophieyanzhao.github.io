# Model Design

Due to serial nature of customer reviews, we use Recurrent Neural Network to model the time dependency of words in each sentences and predict whether the review is positive or negative. We apply LSTM layers instead of regular recurrent neural network because it is more robust to vanishing or exploding gradient problems. We trained an embedding layer to reduce the dimensionality of word representation. Our codes are modified from Subramanian's book: *Deep Learning with Pytorch* RNN chapter. Hyperparameters such as number of layers and hidden dimensions are tuned on our dataset, with careful consideration for the tradeoff between model performance compuational time. 

Our dataset is highly imbalanced, with 12 million 5-rating review, accounting for 59% of the data. To overcome this imbalance, we experimented with multiple loss functions such as MSE(recoding response as continuous), crossEntropy and BCEWithLogitsLoss. We chose loss function to be BCEWithLogitsLoss because it is both numerically stable and has the flexibility to incorporate class weights. We also transformed the reviews to be binary(with 1-3 as negative and 4-5 to be positive) to combat the imbalance problem. The dataset class breakdown before and after this change is reported in details in our data section.