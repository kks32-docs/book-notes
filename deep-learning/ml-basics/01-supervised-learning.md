## Supervised learning

Learning algorithms that learn to associate some input with some output give a training set of examples of input $x$ and output $y$ provided by a "human supervisor". 

### Probabilistic supervised learning

Estimating the probability distribution $p(y|x)$, we can do that using a maximum likelihood estimation to find the best parameter vector $\theta$ for a parametric family of distribution $p(y|x;\theta)$. 

Linear regression can be written as:

$$p(y|x, \theta) = \mathcal{N}(y; \theta^Tx, I)$$

We can generalize linear regression to classification scenarios by defining a different family of probability distributions. If we have two classes (class 0 and class 1), we need to specify the probability of one class (class 0 or 1) because the values of probabilities must add to 1. The normal distribution is parameterized in terms of a mean. Any value of this mean is valid. On the other hand, a distribution over binary variables is more complex because its mean should always be between 0 and 1. Using the logistic sigmoid function to have values between 0 and 1, we can enforce this condition:

$$p(y = 1|x, \theta) = \sigma(\theta^T x)$$

Logistic regression is a misnomer and is not a regression but does classification by minimizing the negative log-likelihood using gradient descent. 

#### Support Vector Machines (SVM)

Similar to logistic regression, SVMs are driven by a linear function $w^T x+ b$. SVMs do not provide probability but only output a class identification. SVM predicts a positive class when $w^Tx + b$ is greater than zero, and a negative class when $w^Tx + b$ is less than zero. 


**Kernel trick** consists of observing many many ML algorithms be written exclusively in terms of dot products between examples. 

$$w^T x + b = b + \sum_{i=1}^m \alpha_i x^T x^{(i)}$$

$x^i$ is the training example and $\alpha$ is the vector of coefficients. Rewriting allows us to replace $x$ with the output of a given feature function $\phi(x)$ and the dot product with the form $k(x, x^i) = \phi(x) \cdot \phi(x^i)$ called a kernel and is the inner product analogous to $\phi(x)^T \phi(x^i)$. 

After replacing the dot product with kernel function, we can make predictions:

$$f(x) = b + \sum_i \alpha_i k(x, x^i)$$

The function is nonlinear with respect to $x$ but the relationship between $\phi(x)$ and $f(x)$ is linear. The kernel-based function is equivalent to preparing the data and applying $\phi(x)$ to all inputs, then learning a linear model in transformed space. 

Kernels enable us to learn models that are nonlinear in $x$ using a convex optimization technique that is guaranteed to converge efficiently. We assume $\phi$ is fixed and optimize $\alpha$. Optimization is in a linear space. The kernel function is significantly less computationally intensive than taking the dot product of $\phi(x)$ vectors. 

The kernel allows us to accommodate infinite dimensions of $\phi(x)$, which are intractable (problems that can be solved in theory, but in practice is not possible due to too much need for the resource) as a tractable problem.  Construct a feature map $\phi(x)$ with non-negative values of $x$. Suppose this returns a mapping of $x$ ones followed by infinitely many zeros, we can write a kernel $k(x, x^{(i)}) = \min (x, x^{(i)})$ that is exactly equal to the corresponding infinite dimension dot product. 

Commonly used kernel is the Gaussian kernel: $k(u, v) = \mathcal{N}(u-v; 0, \sigma^2 I)$ is the _radial basis function _ whose values decrease along lines in $v$ space radially outward from $u$. 

The Gaussian kernel can be thought of as a template. A training example of mapping $x$ to $y$ is a template for $y$ when a new point $x^\prime$ is close $x$ the Gaussian kernel has a large response indicating $x^\prime$ is very similar to $x$ template. The model then puts a large weight on the associated training label $y$. Overall the prediction will combine many such training labels weighted by their similarity of the corresponding training example. 

An algorithm using kernel trick is called _kernel machines_. A major drawback of kernel machine is the cost of evaluating the decision function is linear in the number of examples $(i)$ in $\alpha_i k (x, x^i)$. SVMs mitigate by learning a vector of $\alpha$ that contain mostly zeros. Classifying a new example, therefore, requires evaluating the kernel function only for the training examples that have non-zero $\alpha_i$. Training examples are called _support vectors_.

### k-nearest neighbors

When we want to produce an output $y$ for a new test input $x$, we find the k-nearest neighbors to $x$ in the training data $X$. We then return the average of the corresponding $y$ values in the training set. This works for any supervised learning where we can take the average over $y$ values. For classification, the average can be used to determine the probability distribution over a class. The non-parametric k-nearest neighbor algorithm _cannot learn features_ and is computationally intensive for large datasets and less accurate on small datasets.

### Decision trees

Decision trees break the input space into regions. Each node is associated with a region, and internal nodes break that region into one subregion for each child node. Space is subdivided into non-overlapping regions with a one-to-one correspondence between leaf nodes and input regions. Each leaf node maps every point in its input region to the same output. Each leaf requires at least one training example to define, so the decision tree cannot learn a function with more local maxima than the number of training examples. 