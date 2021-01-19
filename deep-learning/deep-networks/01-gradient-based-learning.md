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

Softmax is useful when defining distributions over a discrete variable with $n$ possible values: $\hat{y}_i = P(y=i|x)$. For a linear layer with unnormalized log probabilities $z = w^T h + b$, $z_i = \log \tilde{P}(y=i|x)$ the softmax function is $softmax(z)_i = \frac{\exp(z_i)}{\sum_j\exp(z_j)}$. Using the maximum log likelihood $\log softmax (z)_i = z_i - \log \sum_j \exp(z_j)$. The first term shows input $z_i$ always has a direct contribution to the cost function. As this term cannot saturate learning can proceed even if the second term becomes too small.