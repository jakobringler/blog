---
title: "Feed Forward Networks (FFNs)"
draft: true
tags:
- machinelearning
---
A feedforward network is a type of artificial neural network where information just moves in one direction—from the input layer, through some hidden layers, and finally to the output layer. Think of it like a one-way street for data.

Here’s the lowdown on how it works:

1. Input Layer: This is where the network gets its data. Each neuron here represents a different feature of that data.

2. Hidden Layers: After the input, the data flows through one or more hidden layers. Neurons in these layers do some math—like applying weights and biases, and then running everything through an activation function (like ReLU or sigmoid). This helps the network learn and transform the data.

2. Output Layer: This is the end of the line. It gives the final output, whether that’s a class in a classification task or a single value for something like regression.

3. Forward Propagation: When you feed data in, it travels through the network, layer by layer, turning the input into an output.

4. Training: To get better, the network goes through a training process. It adjusts the weights based on how wrong its predictions were, using techniques like backpropagation and gradient descent.

So, in a nutshell, a feedforward network is all about moving data from start to finish without any loops, making it great for tasks like figuring out categories or predicting values!

### Architecture

TODO:

aso known as Multilayer perceptron



Related: [[notes/Convolutional Neural Networks (CNNs) |Convolutional Neural Networks (CNNs)]], [[notes/Recurrent Neural Networks (RNNs) |Recurrent Neural Networks (RNNs)]], [[notes/Generative Adversarial Networks (GANs) |Generative Adversarial Networks (GANs)]]

---
sources / further reading:
- 



