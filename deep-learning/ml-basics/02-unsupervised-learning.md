## Unsupervised learning

Unsupervised learning algorithms are those that experience only "features" but not a supervision signal, e.g., density estimation, learning to draw samples from a distribution, learning to denoise data from some distribution.

Find the "best" representation of the data that preserves as much information about $x$ as possible while obeying some constraints or a penalty to keep the representation simpler or more accessible than $x$. There are three categories of representation:
- *lower-dimension representation*: Compresses as much information about $x$ in a small representation. The representation attempts to remove any redundancies resulting in a reduction in dimensions to achieve compression while disregarding less amount of data. 

- *sparse representation*: Embed data into a representation whose entries are mostly zeros for most inputs. This requires increasing the dimension of the representation to become mostly zeros. This distributes the data along the axes of the representative space. 

- *Independent representation*: Attempts to disentangle the source of variation underlying the data distribution such that the dimension of representation are statistically independent. 

### Principal Component Analysis

PCA can be thought of as an unsupervised learning algorithm that learns a representation that has low dimensions than the original input. PCA learns a representation where the elements have no linear correlations with each other. The algorithm must remove the non-linear relationships between variables to achieve full independence. 

PCA learns an orthogonal, linear transformation of the data that projects an input $x$ to a representation $z$ that aligns the direction of the greatest variation with the axes of the new representation space. We could learn a one-dimensional representation that best reconstructs the original data (Mean Square Error), and this representation corresponds to the _first principal component_ of the data.

Consider a design matrix $X$ of size $m\times n$. The data has a mean of zero $\mathbb{E} [x] = 0$. The unbiased sample variance of $X$ is $Var(X) = \frac{1}{m-1}X^TX$. PCA finds a representation (through linear transformation) $z = W^Tx$, where $Var(z)$ is a diagonal matrix $= \frac{1}{m-1}\Sigma^2$. When we project the data $x$ to $z$ via linear transformation $W$, the representation has a diagonal covariance of the matrix $\Sigma^2$ which immediately implies the individual elements of $z$ are mutually uncorrelated. Removing the correlations is one aspect of dependency between elements, to disentangle more complicated forms of feature dependencies, we need more than a simple linear transformation. 


### k-means clustering

The k-means clustering divides the training set into $k$ different clusters of examples that are near each other. The algorithm provides k-dimensional one hot-code vector $\mathbf{h}$ representing an input $x$. If $x$ belongs to cluster $i$, then $h_i = 1$ and all other entries of representation of $\mathbf{h}$ are zero. k-means is a _sparse representation_ with only one element being non-zero. 

The k-means initializes k-different categories $\{\mu^1, \dots \mu^k\}$ and alters between two different steps until convergence. Each training is assigned to cluster $i$, where $i$ is the index of the nearest centroid $\mu^i$. In the other step, each centroid $\mu^i$ is updated to the mean of all training examples $x^j$ assigned to cluster $i$. 

Clustering is ill-posed; there is no single criterion that measures how well a clustering of the data or its representation corresponds to the real world values. The different clustering algorithms can correspond well to some properties of the real world. Consider we have a dataset of red trucks, red cars, gray trucks, and gray cars. Two different clustering algorithms can cluster them based either in terms of the type of vehicle or in terms of their colors. A third clustering algorithm can classify them into four distinct categories, but this loses the feature of similarity between different types. Hence, we prefer distributed representations to one-hot representation and gives us the ability to measure the similarity between objects by comparing many attributes instead of just testing if one attribute matches. 


### Stochastic Gradient Descent (SGD)

A large training set is necessary for good generalization but is computationally expensive. The cost function in ML can be decomposed as a sum over training examples of some per example loss function, e.g., the negative conditional log-likelihood of the training data:

$$J(\theta) = \frac{1}{m} \sum_{i=1}^m L(x^{(i)}, y^{(i)}, \theta)$$

$L$ refers to the loss function $L(x, y, \theta) = -\log p(y|x;\theta)$.

For these additive cost functions, the gradient descent requires

$$\nabla_\theta J(\theta) = \frac{1}{m} \sum_{i=1}^m \nabla_\theta L(x^{(i)}, y^{(i)}, \theta)$$

The computational cost is $O(m)$. Insight from SGD is that that gradient is an _expectation_, which can be approximately estimated using a small set of samples. At each step we can sample a **mini-batch** of examples $\mathbb{B} = \{x^1, \dots, x^{m^\prime}\}$ drawn uniformly from the training set. The $m^\prime$ is smaller than $m$ and typically in a few hundred and remains fixed even as the training set's size increases to billions. 

The estimate of the gradient is:

$$g = \frac{1}{m} \nabla_\theta  \sum_{i=1}^{m^\prime} L(x^{(i)}, y^{(i)}, \theta)$$

The SGD follows the estimated gradient downhill $\theta \leftarrow \theta - \varepsilon g$ and $\varepsilon$ is the learning rate. SGD is widely used in ML training large linear models on very large datasets. 