# Gradient-based learning

The non-linearity of neural networks causes loss function to become non-convex. Hence, there is no convergence guarantee, and the optimization algorithm merely drives the cost function to a very low value, rather than a global convergence. For feedforward network, all parameters are initialized to small random values, and the biases are set to zero or positive values. 

## Cost function
Our parametric model defines a distribution $p(y|x;\theta)$, and we use the cross-entropy between the training data and the model's predictions as the cost function. The total cost function of a neural network will combine the primary cost function with a regularization term (e.g., weight decay).

### Learning conditional distribution with maximum likelihood
In many cases, the cost function is simply the negative log-likelihood, i.e., the cross-entropy between the training data and the model distribution:

$$J(\theta) = - \mathbb{E}_{x, y \sim \hat{p}_{data}} \log P_{model}(y|x)$$

If $p_{model} (y|x) = \mathcal{N}(y; f(x; \theta), I)$ we can recover the MSE:

$$J(\theta) = 0.5  \mathbb{E}_{x, y \sim \hat{p}_{data}} ||y - f(x; \theta)||^2 + const $$


Deriving the cost function from maximum likelihood removes the burden of designing the cost function for each model. Specifying a model $p(y|x)$ automatically determines a cost function $\log p(y|x)$.

The gradients of cost functions must be large and predictable to be a good guide for learning. Functions that saturate (becomes flat) undermines this objective. In many cases, activation functions saturate the outputs (caused by the occurrence of exponential). Negative log-likelihood avoids this problem (log removes these exponential functions). 

### Learning conditional statistic

We can view cost function as a __functional__, a mapping from functions to a real value. We can thus think of our design as choosing a function rather than merely choosing a set of parameters. Calculus of variations shows:

$$f^* = \arg \min_f  \mathbb{E}_{x, y \sim p_{data}} ||y - f(x)||^2$$

which yields:

$$f^*(x) = \mathbb{E}_{y \sim p_{data}(y|x)} [y]$$

If we train on infinitely many samples from the true data generating distribution, minimizing the MSE cost function would give a function that predicts the mean of $y$ for each value of $x$.

Different cost functions yield different statistics:

$$f^* = \arg \min_f  \mathbb{E}_{x, y \sim p_{data}} ||y - f(x)||$$

is the *Mean Absolute Error (MAE)* predicts the median value of $y$ for each $x$. 

Both MSE and MAE often lead to poor results when using gradient-based optimization. Hence, cross-entropy functions are more useful even when it is not necessary to estimate the entire distribution of $p(y|x)$.

## Output units
The choice of the cost function is controlled by choice of output units. Any output unit could also be a hidden unit. For a feedforward network providing a set of hidden features $h = f(x;\theta)$, the role of the output layer is then to provide some additional transformation from the features to complete the transformation that the neural network must perform. 

### Linear units for Gaussian output distribution

Given features $h$, a layer of linear output units produces a vector $\hat{y} = w^T h + b$. Linear output layers are often used to produce the mean of a conditional Gaussian distribution. Then the maximum likelihood is equivalent to minimizing the MSE. Linear units _do not saturate_ and are useful for gradient-based optimization.

### Sigmoidal units for Bernoulli output distribution

Bernoulli distribution is defined by a single number. The neural network needs to predict only $P(y=1|x)$ for this number to be a valid probability; it must lie between [0, 1]. If we use a linear unit with a threshold:

$$P(y = 1|x) = \max \{0, \min\{1, w^Th + b\}\}$$

is a valid conditional distribution but is hard to train. Anytime $w^Th + b$ strayed outside the unit interval, the gradient would be zero, and the learning algorithm will have no guidance to improve the parameters.

A sigmoid output $\hat{y} = \sigma (w^T h + b)$ uses a linear layer to compute $z = w^Th+b$ then uses the logistic sigmoid to convert $z$ to a probability. The variable $z$ defining such a distribution over binary variables is called a **logit**. The unnormalized $\log \tilde{P}(y) = yz$ is both linear in $y$ and $z$. 

$$\tilde{P}(y) = exp(yz) \quad P(y) = \frac{\exp(yz)}{\sum_{y^\prime = 0} \exp(y^\prime z)} = \sigma((2y -1)z)$$

The negative log-likelihood undoes the exponential from the sigmoid function.

$$J(\theta) = - \log P(y|x) = -\log \sigma((2y -1)z) = \zeta ((1-2y)z)$$

By rewriting in terms of the softplus function, we can see it only saturates when $(1-2y)z$ is negative. However, saturation only happens when we already have an answer, i.e., either when $y=1$ and $z$ is positive or $y=0$ and $z$ is very negative. The softplus function doesn't shrink the gradient. If we use other loss functions, such as MSE, the loss can saturate when $\sigma(z)$ saturates. 

### Softmax for Multinoulli distribution

Softmax is useful when defining distributions over a discrete variable with $n$ possible values: $\hat{y}_i = P(y=i|x)$. For a linear layer with unnormalized log probabilities $z = w^T h + b$, $z_i = \log \tilde{P}(y=i|x)$ the softmax function is $softmax(z)_i = \frac{\exp(z_i)}{\sum_j\exp(z_j)}$. Using the maximum log likelihood $\log softmax (z)_i = z_i - \log \sum_j \exp(z_j)$. The first term shows input $z_i$ always has a direct contribution to the cost function. As this term cannot saturate learning can proceed even if the second term becomes too small. The first term encourage $z_i$ to be pushed up, while second term encourages all $z$ be pushed down. $\log \sum_j \exp(z_j) \approx \max_j z_j$ when $\exp(z_k)$ is insignificant for any $z_k$ that is noticeably less than $\max_j z_j$. The intuition is we strongly penalize the most active incorrect prediction. 

Objective function with log works best with softmax to undo the exponential; if not, the models fail to learn. In particular square error is a poor loss function for softmax units even if the model makes highly confident predictions, but the predictions will be incorrect.

Softmax turns a vector of $k$ real values to a vector of $k$ real values that sum to 1. If the input is small or negative, it turns to a small probability; larger values turn into a large probability but will always remain between 0 and 1. Softmax is a softened version of $\arg \max$ and is continuous and differentiable. Softmax is written as:
$$ softmax(z)_i = \frac{\exp^z_i}{\sum_{j=1}^k \exp^z_j}$$

A general stable form of softmax identity is: $softmax(z) = softmax(z - \max_i z_i)$. Softmax is driven by the amount that its arguments deviate from $\max_i z_i$. An output softmax $(z_i)$ saturates to 1, when the corresponding input is maximal $(z_i = \max_i z_i)$. The output can also saturate to 0 when $z_i$ is not maximal, and the maximum is much greater. If the loss function is not designed to compensate the exponent in the softmax, the model will fail to learn. At extreme softmax is the winner-takes-all function, when one of the output is nearly 1, and the others are 0.

### Other output types
Consider that we want to learn the variance of a conditional Gaussian for $y$ given $x$. The maximum log-likelihood estimator of the variance is simply the empirical mean of the square difference between observations $y$ and their expected value. 

In general, we define a conditional distribution $p(y| x; \theta)$ the principle of maximum likelihood suggests we use $- \log p (y|x;\theta)$ as our cost function. We think of neural networks as representing a function $f(x; \theta)$. The output of this function are not direct prediction of the value of $y$, but $f(x;\theta) = w$ provides the parameters for distribution over $y$. The loss function can be interpreted as $-\log p(y; w(x))$.

Gradient computations involving division (such as variance calculations) result in arbitrary steep slopes near zero and affect the learning. 


## Hidden units
### Rectified Linear Units (ReLU)
There are no definitive guiding theoretical principles behind the choice of hidden units. The ReLU is an excellent default choice. Predicting in advance which hidden unit will work best is usually impossible. 

ReLU $g(z) = \max {0, z}$ is not differentiable at $z=0$, but gradient descent still performs well enough for ML tasks because the neural network training algorithms do not usually arrive at a local minimum, but instead simply reduce its value significantly. Most hidden units can be described as accepting a vector of input $x$, computing an affine transformation $z = W^T x + b$ and then applying an element-wise non-linear function $g(z)$. Most hidden units differ on the choice of the activation function $g(z)$. 

ReLU is similar to linear units, so easy to optimize and outputs zero across half its domain that makes the derivatives through a ReLU remain large whenever the unit is active. Derivatives of ReLU is one everywhere the unit is active; the gradient direction is far more useful for learning and does not introduce any second-order effects.  $h = g(W^Tx + b)$, usually setting $b$ to be a small positive value of 0.1. The drawback of ReLU is it cannot learn with gradient-based methods on examples for which their activation is zero.

Generalize ReLU is written as: when $z_i < 0$, $h_i = g(z, \alpha)_i = \max(0, z_i) + \alpha_i \min(0, z_i)$. When $\alpha_i$ is set to 0.01 it is called **LeakyReLU** and the parametric ReLU or **PReLU** is when $\alpha_i$ is a learning parameter. 

### Maxout units
Maxout units are generalize ReLU. Instead of applying an element-wise function $g(z)$, maxout units divide $z$ into groups of $k$ values and each maxout unit then outputs the maximum element of one of the groups:

$$g(z)_i = \max_{j \in G^{(i)}} z_j$$

Maxout provides a way of learning piece-wise linear function that responds to multiple directions in input $x$. 

*Catastrophic forgetting* is when neural networks forget how to perform tasks that they were trained on in the past. 

### Logistic sigmoid and hyperbolic tangent

$$g(z) = \sigma(z)$$ 

The sigmoid function is output units to predict the probability that an arbitrary variable is 1. Sigmoid saturates over a wide range and is strong near zero. Difficult to use with gradient-based learning

$$g(z) = \tanh(z) = 2\sigma(2z) - 1$$

Hyperbolic tangent performs better than sigmoid functions because $\tanh(0)=0$ while $\sigma(0)=0.5$. Sigmoid functions are more common in models other than feedforward networks, such as the recurrent networks and other probability models.

### Other hidden units

Having no activation $g(z)$ is also a possibility where the layer is purely linear. Linear hidden units are an efficient way of reducing the number of parameters in a network. 

Other common hidden units are:

- _Radial Basis Function_: Saturates to 0 for most values of $x$ and is difficult to optimize
- _Softplus function_: Smooth version of the rectifier. It is generally discouraged to use the softplus function as it generates counterintuitive results and empirically performs worse than ReLU, although it is differentiable everywhere, unlike ReLU.
- _Hard tan_: Similar to $\tanh$ and rectifier but is bounded $g(a) = \max(-1, min(1, a))$.


## Architecture

Architecture refers to the overall structure of the network: the number of units and how they are connected to each other. Neural networks are typically organized into groups of units called __layers__, with each layer being a function of the layer preceding it like a chain structure. The first layer is written as $h^{(1)} = g^{1} ({W^{(1)}}^Tx + b^{(1)})$ and the second layer as $h^{(2)} = g^{2} ({W^{(2)}}^Tx + b^{(2)})$. The main architecture considerations are the depth and the width of each layer. Deeper units often use far fewer units per layer and far fewer parameters. _The ideal network architecture for a task must be found via experimentation guided by monitoring the validation set error_.

## Universal approximation theorem

A feedforward network with a linear output layer and at least one hidden layer with any "squashing" activation function (such as logistic sigmoid) can approximate any Borel measurable function from one finite-dimensional space to another with any desired non-zero amount of error, provided there are enough hidden units. Any continuous function in a bounded subset of $\mathbb{R}^n$ is Borel measurable. 

The property of universal approximation in feedforward networks allows one to represent nonlinear functions using linear models. Although MLP can represent nonlinear functions, the learning may still fail either because the optimization algorithm is unable to find the desired parameters or the training algorithm chooses the wrong function. For any function, there exists a feedforward network that approximates the function. However, there is no universal procedure for examining a training set of specific examples and choosing a function that will generalize to points, not in the training set. The number of hidden units in a shallow model is exponential in $n$, i.e., $O(2^n)$. 

Although a single layer feedforward network, in theory, is sufficient to represent any function, the layer may be infeasibly large and may fail to learn and generalize correctly. The deeper model can reduce the number of units and the amount of generalization error. Choosing a deep model implies the function we want to learn should involve the composition of several simple functions. Empirically, however, greater depth doesn't seem to translate to better generalization for a wider variety of tasks. Using deeper architecture suggests that the architecture expresses a useful prior over the space of functions the model learns. Increasing the number of parameters without increasing the depth is not nearly as effective in increasing the test set performance. The function should consist of many simple functions composed together. Overfitting on shallow models can be tested by increasing the number of parameters and measure the test accuracy.