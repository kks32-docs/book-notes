# Deep Learning: Introduction
## Introduction to machine learning
Machine Learning (ML) involves acquiring knowledge by extracting patterns from raw data. An ML algorithm's performance depends on the data and its representation used to train the model (input). Hence, identifying the relevant data and its representation to train a model requires domain-specific expert knowledge. Each piece of information (data) included in the representation is defined as a _feature_.

## Representation learning 
Information can have different representations. Two data clusters that require delineation can be represented as a scatter plot either on Cartesian coordinates or polar coordinates. These two distinct representations highlight different features of the data. Representation learning not only learns the mapping between the representation and the output but also the representation itself (e.g., autoencoder #unclear). 

### Problems of representation learning
When writing algorithms to identify features, the goal is to recognize different sources of variations (observable or unobservable quantities that help us make sense of the rich variations in the data). In real-world applications, it's hard to identify these factors. Furthermore, the factors influence every single piece of data. Hence, it's hard to disentangle the factors from the data. 

## Deep learning (DL)
Deep learning (DL) solves the central issue of representation learning  (see [[00-introduction#Problems of representation learning]]). The idea behind deep learning is to build complex concepts out of simpler ones. Let's consider an example of object identification in an image. In the example of object identification, the input image is the visible layer. An image is a functional mapping of a set of pixel values to an object (which we observe). The deep learning algorithm consists of a series of hidden layers (unobservable) that extract increasingly abstract features from the image, culminating in an output (object identification). Each layer performs a functional mapping with a layer's output often used as an input to the subsequent layer. In an image, the first layer could identify edges (based on the contrast between neighboring pixels), used as an input to the second layer. The second layer uses these edges to extract contours and corners, facilitating identifying object parts in the subsequent layers. As these layers' functional mapping cannot be observed, the layers are called hidden layers in a deep learning algorithm. Not all nodes in a layer necessarily encode (activate) useful information from the input. Some nodes in a layer may only store state information (think of pointers in computer memory), which has nothing to do with the input but helps to organize processing. 


### Perspectives of DL
There are two main perspectives of DL:
- Identifying the right representation of the data
- Depth of a neural network influence its learning ability. The greater the depth, the greater the power to refer back to earlier results. 

The depth of a neural network could represent the number of sequential operations or the graph's depth describing the model. A model's depth can vary even for the same architecture (a computational problem at hand) based on representation choice. The representation could be individual operations in a computation (addition, multiplication, sigmoid function) or a concept (logistic regression). Like the choice of programming language controls the number of lines required to perform a computation, the choice of representation (learned functions or learned concepts) influences the model's depth. Although consensus has not been reached if the depth is a measure of the number of sequential computational operations or the layers in a graph describing the model, the depth of a model controls the learning of a model. 

### Neural perspective of DL

Deep Learning is concerned about building computer systems that can successfully solve tasks that require intelligence. DL started with the neural perspective of reproducing the computational aspect of the brain to recreate intelligent behavior. The other perspective of DL is to understand the brain and principles behind human intelligence. 

DL goes beyond the neural perspective to a more general principle of learning _multi-level composition_. Early example is a linear model that learns a linear mapping of $n$ input values $x_1, \dots x_n$ to an output value $y$. The model learns a set of weights $w_1, \dots w_n$ and computes $f(x, w) = x_1 w_1 + x_2 w_2 + \dots x_n w_n$. Linear models remain some of the widely used DL algorithms, but have certain limitations. The popular limitation of the linear model is its inability to learn the XOR function:

$$
f([0, 1], w) = 1 \quad f([1, 0], w) = 1 \quad f([1, 1], w) = 0
$$

Although DL started from neuroscience, it's no longer the prominent guide for the field due to our lack of detailed understanding of how thousands of neurons interact in our brain. However, neuroscience gives hope that a single DL algorithm can potentially solve different tasks, based on neuroscience research on ferrets that learned to "see" with their brain's auditory processing region. Similar to interactions of neurons in human brains, a large number of simple computational units can achieve intelligent behavior, which is an inspiration to the hidden layers in the DL networks. Nevertheless, human brains have neurons that do very complex functions, while the neurons of modern neural networks use simplified Rectified Linear Units (ReLU). We are still a long way to generalize DL algorithms for different tasks. 

An individual neuron or a small collection of neurons is not particularly useful. When a large number of neurons work together, it can be intelligent. Unlike biological neurons, DL networks are densely connected. However, the number of neurons has remained astonishingly small until recently. The current trend in ANNs show, reaching the number of neurons as humans can only be achieved by 2050. Even then, the DL neurons may not be able to perform complex tasks as the biological neurons.

#distributed_representation
Another critical aspect of learning features is **distributed representation**. Each input should represent many features. Each feature should be responsible for representing many possible inputs. For example, our vision system needs to recognize three objects (trucks, cars, and birds) and three colors (red, green, and blue). We can use a total of 9 individual neurons to represent all possible combinations (red truck, red car...blue bird) or leverage distributed representation and use six neurons (three to represent the objects and three for the colors). 

## Data Set Requirements 
#data_requirements, #supervised_learning

The recent gain in popularity of DL is due to the improvements in the computational ability and a large amount of data availability. The key burden of statistical estimation is how to generalize well to new data after observing a small amount of data. Machine learning has been able to address this key statistical issue. As of 2016, supervised learning algorithms can provide acceptable results with 5000 labeled data per category and will match or exceed human performance with a dataset containing 10 million labeled examples.

## Multi Layer Perceptron (MLP) 
#mlp
MLP is a mathematical function mapping some set of input values to output values. The MLP function is formed of a simpler function that provides a new representation of the input. 

## Reinforcement learning
#reinforcement_learning

Reinforcement learning is when an autonomous agent learns to perform a task by trial and error without human interference.