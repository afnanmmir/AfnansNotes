---
title: Backpropagation
tags:
- software
- machine learning
- academic
---
Before reading this, make sure to read the piece on [Gradient Descent](GradDesc.md), as this will build off of that piece.

# Motivation
From the gradient descent article, we saw that given input data $X$ and output data $Y$, we can approxiamate parameter values/matrices such as $W$ and $b$ that would best map data points in $x_i$ to their corresponding outputs $y_i$ using an iterative algorithm that would aim to minimize a loss/cost function. We would find the gradient of the cost functions with respect to our parameters (in this case $W$ and $b$), and slowly descend towards a local minimum with the following equation:
$$\Theta_{new} = \Theta_{old} - \alpha\nabla(J(\Theta))$$
where $\alpha$ is a hyperparameter of the learning rate. Computing the gradient for each parameter in this case does not require much effort, as there are no complex functions, and there aren't a significant amount of parameters to account for.
However, consider the following picture:
![Neural Network](imgs/neuralnet.png)

Here we see 4 layers: the input, 2 hidden layers, and the output layer. Each component of the input layer is sent to all 4 of the nodes in the first hidden layer, and each node in the first hidden layer is passed to each node in the second hidden layer etc. At each node, except for the input, the input data into the node is first transformed linearly, similar to $Wx + b$. However, the input is then transformed with some nonlinear function. We can see here that the amount of parameters greatly increases as our machine learning architectures become more complex. Each node in hidden layer 1 has weights associated with each input node, and each node in hidden layer 2 has weights associated with each node in hidden layer 1. Additionally, the nonlinearities adds another level of complexity. We can see that as the compelxity of the architecture increases, it becomes more and more computationally expensive to go and calculate the gradient of every single parameter of the network individually. So, how can we do this in an efficient way such that we don't lose as much time calculating all the gradients.

# The Chain Rule
Before we get into the algorithm, we visit a topic from calculus that is the foundational building block of the algorithm: the chain rule. Say we have a function $f(x)$, and then a function $g(f(x))$, and we wanted to find the derivative of $g$ with respect to $x$. We would first take the derivative of $g$ with respect to $f$, and then multiply it by the derivative of $f$ with respect to $x$. In other words:
$$\frac{\mathrm{d}g}{\mathrm{d}x} = \frac{\mathrm{d}g}{\mathrm{d}f} * \frac{\mathrm{d}f}{\mathrm{d}x}$$
As you can, when we compound functions, we begin multiplying derivatives in order to calculate the full derivative. We can leverage this when calculating gradients for deep neural networks.
# Applying the Chain Rule to Neural Network Gradient Descent
Consider the following neural network:
![NeuralNet](/content/notes/academics/imgs/neuralnet.png)
We have 4 total layers: one input layer, 2 hidden layers, and one output layer. The inputs normally are multidimensional feature vectors. This feature vector is fed into each node in the first hidden layer. In each node of the first hidden layer, there exists a set of parameters $W$ and $b$. These parameters allow us to perform a linear transformation on the feature vector $x$, creating $z = Wx + b$. Since $x$ is multidimensional, and each input node is connected to each hidden layer node, $W$ and $b$ both end up being matrices containing multiple parameters. After performing the linear transformation, the hidden layer performs a nonlinear transfer function $y = h(z)$. This becomes the output of the hidden layer. It also becomes the input of the next hidden layer. In the next hidden layer, the same process takes place, but with different $W$ and $b$ matrix values. This is repeated until we get to the output layer, where we finally get the final values.

Clearly with the fully connected network of neurons, and all the weights and biases associated with each neuron, and the nonlinearity applied at each neuron, calculating the gradients of each parameter would be a very cumbersome and computationally expensive process. However, we can greatly expedite this process using the chain rule. Going through the network, we can see that all we are really doing is repeatedly compounding functions onto the original input layer. We take the input, we call $x_0$, and we pass it into the first hidden layer, where we perform a linear transformation:
$$ z_1 = W_1x_0 + b_1 $$
We then take this result, and perform a nonlinear transfer function on it to get the output of the layer:
$$ x_1 = h(z_1) $$
This also becomes the input of the next layer. We continuously compound more and more functions on top, producing more parameters the more layers we have. 

This situation lends itself to the chain rule naturally, and we will be able to leverage the chain rule to more efficiently calculate the gradients.
# The Algorithm