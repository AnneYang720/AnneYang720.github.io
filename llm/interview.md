# Basic Deep Learning Interview Questions and Answers

## Question 1-10 is related to cnn&backpropodation.ipynb

1. **What do you understand by learning rate in a neural network model? What happens if the learning rate is too high or too low?**

Learning rate is one of the most important configurable hyperparameters used in the training of a neural network. The value of the learning rate lies between 0 and 1. Choosing the learning rate is one of the most challenging aspects of training a neural network because it is the parameter that controls how quickly or slowly a neural network model adapts to a given problem and learns. A higher learning rate value means that the model requires few training epochs and results in rapid changes while a smaller learning rate implies that the model will take a long time to converge or might never converge and get stuck on a suboptimal solution. Thus, it is advisable not to use a learning rate that is too low or too high but instead a good learning rate value should be discovered through trial and error.

2. **Can you train a neural network model by initializing all biases as 0?**

Yes, there is a possibility that the neural network model will learn even if all the biases are initialized to 0.

3. **Can you train a neural network model by initializing all the weights to 0?**

No, it is not possible to train a model by initializing all the weights to 0 because the neural network will never learn to perform a given task. Initializing all weights to zeros will cause the derivatives to remain the same for every w in W [1] because of which neurons will learn the same features in every iteration. Not just 0, but any kind of constant initialization of weights is likely to produce a poor result.

4. **What has fostered the implementation and experimentation of powerful neural network architectures in the industry?**

- Flexibility makes deep learning powerful. Neural networks are universal function approximators so even if it is a complex enough problem at hand(where the formula between input and output is not known), a neural network can be approximated. 
- Also, transfer learning (where the trained weights of an existing neural network can be used to initialize the weights of another network that performs similar tasks) makes the application of deep learning much easier under situations when training a neural network from scratch is costly or almost impossible when there is data scarcity.
- Faster and powerful computational resources are also a prime reason for the adoption of neural network architectures. One cannot deny the fact that it is faster to train a neural network in just minutes with GPU acceleration which would otherwise take days for the network to learn.

5. **Can you build deep learning models based solely on linear regression?**

Yes, it is definitely possible to build deep networks using a linear function as the activation function for each layer if the problem is represented by a linear equation. However,   a problem that is a composition of linear functions is a linear function and there is nothing extraordinary that can be achieved with the implementation of a deep network because adding more nodes to the network will not increase the predictive power of the machine learning model.

6. **When training a deep learning model you observe that after a few epochs the accuracy of the model decreases. How will you address this problem?**

The decrease in the accuracy of a deep learning model after a few epochs implies that the model is learning from the characteristics of the dataset and not considering the features. This is referred to as the overfitting of the deep learning model. You can either use dropout regularization or early stopping to fix this issue. Early stopping as the phrase implies stops training the deep learning model any further the moment you notice a drop inaccuracy of the model. Dropout regularization is a technique wherein a few nodes or output layers are dropped so that the remaining nodes have varying weights.

7. **What is the impact on a model with an improperly set learning rate on weights?**

With images as inputs, an improperly set learning rate can cause noisy features. Having an ill-chosen learning rate determines the prediction quality of a model and can result in an unconverged neural network.

8. **What do you understand by the terms Batch, Iterations, and Epoch in training a neural network model?**
- batch: the number of training examples utilized in one iteration
- iterations: the number of batches needed to complete one epoch
- epoch: the number of times the complete dataset is passed forward and backward through the neural network

9. **Is it possible to calculate the learning rate for a model a priori?**

For simple models, it could be possible to set the best learning rate value a priori. However, for complex models, it is not possible to calculate the best learning rate through theoretical deductions that can actually make accurate predictions. Observations and experiences do play a vital role in defining the optimal learning rate.

10. **What are the commonly used approaches to set the learning rate?**
- Using a fixed learning rate value for the complete learning process.
- Using a learning rate schedule
- Making use of adaptive learning rates
- Adding momentum to the classical SGD equation.

11. **Is there any difference between neural networks and deep learning?**

Ideally, there is no significant difference between deep learning networks and neural networks. Deep learning networks are neural networks but with a slightly complex architecture than they were in 1990s. It is the availability of hardware and computational resources that has made it feasible to implement them now.

12. **You want to train a deep learning model on a 10GB dataset but your machine has 4GB RAM. How will you go about implementing a solution to this deep learning problem?**

One of the possible ways to answer this question would be to say that a neural network can be trained by loading the data into the NumPy array and defining a small batch size. NumPy doesn’t load the complete dataset into the memory but creates a complete mapping of the dataset. NumPy offers several tools for compressing large datasets that can be integrated with other NN packages like PyTorch, TensorFlow, or Keras.

13. **How will the predictability of a neural network impact if you use a ReLu activation function and then use the Sigmoid function in the final layer of the network?**

The neural network is likely to predict one class for all types of inputs because the output of a ReLu activation function is always a non-negative result.

14. **What are the limitations of using a perceptron?**

A major drawback to using a perceptron is that they can only linearly separable functions and cannot handle non-linear inputs.

15. How will you differentiate between a multi-class and multi-label classification problem?

In a multi-class classification problem, the classification task has more than two mutually exclusive classes whereas in a multi-label problem each label has a different classification task, however, the tasks are related somehow. For example, classifying a set of images of animals which may be cats, dogs, or bears is a multi-class classification problem that assumes that each sample has only one label meaning an image can be classified as either a cat or a dog but not both at the same time. Now imagine that you want to process the below image. The image shown below needs to be classified as both cat and dog because the image shows both the animals. In a multi-label classification problem, a set of labels are assigned to each sample and the classes are not mutually exclusive. So, a pattern can belong to one or more classes in a multi-label classification problem.

16. What do you understand by transfer learning?

You know how to ride a bicycle, so it will be easy for you to learn to drive a bike. This is transfer learning. You have some skill and you can learn a new skill that relates to it without having to learn it from scratch. Transfer learning is a process in which the learning can be transferred from one model to another without having to make the model learn everything from scratch. The features and weights can be used for training the new model providing reusability. Transfer learning works well in training a model easily when there is limited data.

17. What is fine-tuning and how is it different from transfer learning?

In transfer learning, the feature extraction part remains untouched and only the prediction layer is retrained by changing the weights based on the application. On the contrary in fine-tuning, the prediction layer along with the feature extraction stage can be retrained making the process flexible.

18. Why do we use convolutions for images instead of using fully connected layers?

Each convolution kernel in a CNN acts like its own feature detector and has a partially in-built translation in-variance. Using convolutions lets one preserve, encode and make use of the spatial information from the image, unlike fully connected layers that do not have any relative spatial information.

19. Can you name a few data structures that are commonly used in deep learning?

You can talk about computational graphs, tensors, matrices, data frames, and lists.

Let the FOMO kick in! Explore ProjectPro's Data Science Project Ideas Repository to start exploring the exciting domain of Data Science today!

20. What are the benefits of using batch normalization when training a neural network?

Batch normalization optimizes the network training process making it easier to build and faster to train a deep neural network.

Batch normalization regulates the values going into each activation function making activation functions more viable because non-linearities that don’t seem to work well become viable with the use of batch normalization.

Batch normalization makes it easier to initialize weights and also allows the use of higher learning rates ultimately increasing the speed at which the network trains.

21. How do you bring balance to the force when handling imbalanced datasets in deep learning?

It is next to impossible to have a perfectly balanced real-world dataset when working on deep learning problems so there will be some level of class imbalance within the data that can be tackled either by –

Weight Balancing -

Over and Under Sampling

22. What do you understand by Gradient Clipping?

Gradient Clipping is used to deal with the exploding gradient problem that occurs during the backpropagation. The gradient values are forced element-wise to a particular minimum or maximum value if the gradient has crossed the expected range. Gradient clipping provides numerical stability while training a neural network but does not provide any performance improvements.

