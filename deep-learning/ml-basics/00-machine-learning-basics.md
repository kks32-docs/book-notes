# Machine Learning Basics

A computer program is said to learn from experience $E$ with respect to some task $T$ using a performance measure $P$, if its performances at tasks in $T$, as measured by $P$ improves with its experiences in $E$.

ML allows us to tackle tasks that are too difficult to solve with fixed programming is written and designed by human beings. Developing an understanding of ML enables us to understand the underlying principles of intelligence. 

## Tasks $T$
An example is a collection of features represented as a vector $x \in \mathbb{R}^n$, where each $x_i$ is a feature. A task $T$ is how ML should process an example. 

**Classification** involves identifying which of the set of $k$ categories the input belongs. The learning algorithm produces a function $\mathbb{R}^n \rightarrow \{1, \dots k\}$ to generate an output $y = f(x)$. Given an input vector $x$, it identifies the numerical code $y$ ($y$ could also be a probability density function). Classification can also be done with missing input by varying the subset of missing data. 

**Regression** predicts a numerical value given some input function: $\mathbb{R}^n \rightarrow \mathbb{R}$. Similar to classification, but the output is different. For example, predicting an expected claim amount. 

**Transcription** involves transforming unstructured data into discrete textual form. (e.g., OCR and speech recognition).

**Structured output** is any task where the output is a vector. In Natural Language Processing (NLP), a sentence is divided into a tree of grammatical structures with nodes (verb, nouns, adverbs, and so on). Another example is the piecewise segmentation of an image, where each pixel has an associated category, which is used in image captioning. 

**Anomaly detection** involves flagging objects as having unusual or atypical behavior (e.g., credit card fraud detection).

**Synthesis and sampling**: Generate new examples that are similar to those in the training data. In these ML algorithms, there is no single correct output, and we expect large variations in the output (e.g., speech synthesis). 

**Imputing missing values**: Given $x \in \mathbb{R}^n$ with some entries missing in $x_i$ of $x$. The algorithm must provide a prediction of values of missing entries. 

**Denoising**: Given a corrupted example $\hat{x} \in \mathbb{R}^n$ obtained by an unknown corruption process form a clean example $x \in \mathbb{R}^n$. Estimates the conditional probability $p(x | \hat{x})$.

**Density estimation** involves learning a probability distribution $p^{model}(x): \mathbb{R}^n \rightarrow \mathbb{R}$. Learn the structure of the data, such as regions of likely and unlikely clusters.

## Performance $P$

To design an ML algorithm, we must quantitatively measure its performance $P$ to carry out a specific task $T$.  In ML algorithms such as classification, classification with missing information, and transcriptions, we can measure the performance in terms of __accuracy__ (proportion of examples for which model produces the correct output) or __error rate__ (proportion of examples for which model produces the incorrect output). The error rate is expressed as $0-1$ loss, where 0 is correct, and 1 is incorrect. When dealing with continuous variables, such as density estimation, the performance is measured as the average log probability the model assigns to a given example. 

We are interested in how well ML performs on datasets it has not seen. We therefore evaluate the performance on a __test set__, separate from the data used to train the learning system. 

The choice of performance measure must correspond well to the desired behavior of the system. Choosing the appropriate performance metric may be difficult and is dependent on the problem. For example, we can measure the performance of a regression using either the size of medium errors or the frequency at which it makes larger errors, the choice depends on the application. 


## Experience $E$

ML can be broadly classified as __supervised__ or __unsupervised__ by the kind of experience allowed to have during the learning process. 

**Unsupervised learning** experience a dataset containing many features, then learn useful properties of the structure of this dataset (e.g., clustering). Unsupervised learning learns the probability distribution either implicitly or explicitly. 

**Supervised learning** experience datasets containing features, but each example is associated with a label or target. Observing several examples of random vector $x$ and associated values of $y$, unsupervised learning estimates the probability $p(y|x)$. 

The classification of supervised or unsupervised is not completely formal definitions. 

Some learning does not just experience a fixed dataset, __reinforcement learning__ interacts with the surrounding environment, and there is feedback between learning systems and their experiences. 

__Design matrix__ is a representation of examples in each row of the matrix, and the columns correspond to the features. 

## Linear Regression

Take a vector $x \in \mathbb{R}^n$ as input and predict $y \in \mathbb{R}^n$ as its output. Let $\hat{y}$ is the value our model predicts $y$ should take on as $\hat{y} = w^T x$, where $w \in \mathbb{R}^n$ is a vector of parameters. $w$ is the vector of values that control the behavior of the system (think of it as a set of weights). If a feature $x_i$ receives a positive weight of $w_i$, increasing the value of the features increases the value of our prediction $\hat{y}$.  If a feature $x_i$ receives a negative weight of $w_i$, increasing the value of the features decreases the value of our prediction $\hat{y}$. If a feature has no weight (zero), then it has no effect on the prediction. 

Our task $T$ predicts $y$ from $x$ by estimating $\hat{y} = w^T x$. Now we need a performance metric $P$. Consider a test set $X^{(test)}, Y^{(test)}$ with values of regression for input $x$, the __Mean Square Error (MSE)__ is written as:

$$MSE = \frac{1}{m}\sum_i(\hat{y}^{(test)} - y^{(test)})^2 = ||\hat{y}^{(test)} - y^{(test)}||_2^2$$

The error in prediction is zero if $\hat{y}^{(test)} = y^{(test)}$. The error increases when the distance between prediction and target increases. 

We need to design an algorithm that produces a set of weights $w$ in a way that reduces the MSE. One way is to minimize the MSE. 

$$\nabla_w MSE_{train} = \frac{1}{m} \nabla_x ||\hat{y}^{(train)} - y^{(train)}||_2^2 = \frac{1}{m} \nabla_x ||X^{(train)} w - y^{(train)}||_2^2 = 0$$

Rewriting and differentiating with respect to $w$ yields:

$$w = (X^{(train)}^T X^{(train)})^{-1} X^{(train)}^T y^{(train)}$$

These weights are computed from the training set to estimate $\hat{y}$.  However, we often deal with $\hat{y} = w^T x + b$, the mapping is still linear, but a mapping from feature to prediction now is an affine function. The intercept term $b$ is called as the __bias parameter__. The output of the system is biased towards being $b$ in the absence of any input, hence it is termed as the bias parameter and is _different from the statistical bias_.

## Generalization

**Generalization** is the ability to perform well on unseen data. The ability to generalize is measured using errors in the ML prediction. _Training error_ computes the error on the training set. _Generalization or test error_ is the expected value of error on new input, typically drawn from a distribution of all possible inputs. 

> How can we affect the performance of a test set when we only observe the training set?

If the training and test set are chosen arbitrarily, it is impossible to control the performance. However, the following assumptions help in how to choose the datasets to improve the performance of ML. The examples in the training and test datasets are collected _independently_. The distribution of examples in the training set and test set are _identically distributed_. The expected training error of a randomly selected model is equal to the expected test error of that model. Factors determining how well an ML algorithm performs on its ability to:
- make training errors small
- make the gap between training and test error small.

**Underfitting** occurs when the model is not able to obtain a sufficiently low error on the training set. **Overfitting** occurs when the gap between training error and the test error is too large. We can control whether a model is likely to overfit or underfit by altering its **capacity**, i.e., the ability to fit a wide variety of functions (altering the number of input features). Models with low capacity struggle to fit, while models with high capacity memorize properties of the training set resulting in overfitting. Capacity is controlled by choosing the _hypothesis space_, the set of functions that can be chosen as a solution.

ML performs best when their capacity is appropriate for the true complexity of the task they need to perform. In practice, ML doesn't find the best function, but the one that significantly reduces the error, meaning the effective capacity may be less than the _representational capacity_ (the ability to choose functions from a family of functions).

_Occam's razor_: Among competing hypotheses that explain known observations equally well, we should choose the "_simplest_" one. 

Statistical learning shows that the discrepancy between training error and generalization error is bounded from above by a quantity that grows as the model capacity grows but shrinks as the number of training examples increases. However, in practice, it is difficult to determine the capacity of DL algorithms, as it is limited by the capabilities of the optimization algorithms. 

_Parametric models_ learn a function described by a parameter vector, whose size is finite and fixed before any data is observed (e.g., linear regression). _ Nonparametric models_ have no such limitations (e.g., a model that predicts the output based on the nearest neighbor input). 

The ideal model is an _oracle_ (true probability distribution). The error incurred by an oracle making a prediction from the true distribution $p(x, y)$ is called *Bayes error*. ML promises to find the rules that are _probably_ correct about _most_ members of the set they concern.

## No free lunch
When averaged over all possible data generating distributions, every classification algorithm has the same error rate when classifying previously unobserved samples. In some sense, no ML algorithm is universally better than any other. A sophisticated algorithm has the same _average performance_ as the one merely predicting that every point belongs to the same class. Our goal is not to seek a universal learning algorithm. Instead, our goal is to understand what kinds of distributions are relevant to the "real world" that an ML agent experiences. 

## Regularization

We can improve a model using a preferential function. Given two functions, ML will choose the preferential functions given the same error. For an algorithm to choose the non-preferred function, the error should be significantly less. In the case of linear regression, we can include preference by adding a _weight decay_ function.

$$J(w) = MSE_{train} + \lambda w^T w$$

$\lambda$ represents the strength of preference to smaller weights. A $\lambda$ of zero refers to no preference, a high value of lambda means biased to a constant slope, and a value approaching zero would reduce the preference. By minimizing $J(w)$, the choices of weights make a trade-off between fitting the training data and being small. More generally, we can regularize a model that learns a function $fn(x, \theta)$ by adding a penalty called a **regularization** to the cost function: $\Omega (w) = w^T w$.  Regularization is any modification we make to a learning algorithm that is intended to reduce its generalization error, but not its training error. Expressing a preference for any function over another is a more general way of controlling the model capacity than altering the _hypothesis space_.

## Hyperparameters
Hyperparameters are the settings used to control the algorithm behavior (for e.g., weight decay $\lambda$ in linear regression [[#Regularization]] or the degree of a polynomial in a regression fit - this is _capacity hyperparameter_). Sometimes a setting is chosen as a hyperparameter because the setting is too difficult to optimize, or the parameter is not appropriate to learn on the training set. This applies to all _capacity hyperparameter_, as they always choose the maximum possible capacity (overfitting). 

It is important that the test examples are not used in making choices of the model, including the hyperparameter. **Validation set** is constructed from the training set by splitting the training set into two disjoint subsets, of which one is used for learning the parameters, while the other is used to estimate the generalization error (validation set), allowing for updating the hyperparameters. The validation set is a subset used to guide the selection of hyperparameters. Typically 80% of the training dataset is used for training, and 20% is used for validation. Since the validation is used to "train" the hyperparameter space, the validation set will underestimate the generalization error. After all optimization, the generalization error may be estimated using the test set.

## Estimators, Bias, and Variance

### Estimators
Estimator is the best prediction of some quantity of interest. Point estimation of a parameter $\theta$ (true value) is $\hat{\theta}$:

$$\hat{\theta}_m = g(x^1, \dots x^m)$$

Function estimation refers to the estimation of the relationship between the input and target variables (is an example of point estimation).

### Bias
$$bias (\hat{\theta}_m) = \mathbb{E}(\hat{\theta}_m) - \theta$$

An estimator is unbiased if $bias(\hat{\theta}_m) = 0$.  An estimator is asymptotically unbiased if $\lim{m \to \infty} bias(\hat{\theta}_m) = 0$ or $\lim{m \to \infty} \mathbb{E}(\hat{\theta}_m) = \theta$. 

### Variance
Variance is how much we expect an estimator to vary as a function of the data sample. $Var(\hat{\theta}_m)$ provides a measure of how we would expect the estimate we compute from data to vary as we independently resample the dataset from the underlying data generation process. 

Neither the square root of sample variance nor the square root of an unbiased estimator of the variance provides an unbiased estimate of the standard deviation. Both approaches tend to _underestimate the true standard deviation_, but are still used in practice. The square root of the unbiased estimator of the variance is less of an underestimate. For large samples, the approximation is reasonable. 

The standard error of mean is instrumental in ML to estimate the generalization error by computing the sample mean error on the test.  The variance of an estimator decreases as a function of $m$, the number of samples in a dataset. 

#### Variance and Bias
Bias estimates the expected deviation from the true value of the function or parameter. Variance provides a measure of deviation from the expected estimator value than any particular sampling is likely to cause. 

**Bias decreases with capacity and influences the underfitting zone, while variance increases with capacity and affects the overfitting zone.**

$$ MSE_{estimator} = \mathbb{E}\left[(\hat{\theta}_m - theta)^2\right] = Bias(\hat{\theta}_m)^2 + Var(\hat{\theta}_m)$$

Desirable estimators keep bias and variance somewhat in check.

#### Consistency
As the number of data points $m$ increases our point estimates converge to the true value of the corresponding parameters $plim_{m\to\infty} \hat{\theta}_m = \theta$.  Consistency ensures that the bias introduced by the estimator diminishes as the number of data examples grows.

### Maximum Likelihood Estimator

Maximum Likelihood Estimator can be viewed as minimizing the dissimilarity between the empirical distribution $\hat{p}_{data}$, defined by the training set and the model distribution with the degree of dissimilarity between the two measured by the KL divergence.

$$D_{KL}(\hat{p}_{data} || p_{model}) = \mathbb{E}_{x\approx \hat{p}_{data}}\left[\log \hat{p}_{data} (x) - \log p_{model} (x)\right]$$

The first part of the equation is only a function of the data generation process, not the model. We train the model to minimize the KL divergence. 

Maximum Likelihood can be thought of as an attempt to make the model distribution match the empirical data $\hat{p}_{data}$. Ideally, we want to match the true data-generation distribution $p_{data}$ but we have no direct access to this distribution. Maximum likelihood is useful to describe the error estimation, maximum likelihood shows that the $MSE_{train}$ is a good measure of error for estimating linear regression.

Maximum Likelihood estimator has the property of _consistency_, as the number of training examples approaches infinity, the maximum likelihood estimate of a parameter converges to a true value if and when:
- The true distribution $p_{data}$ lies withing the model family $p_{model}(:, \theta)$. Otherwise, no estimator can recover $p_{data}$. 
- The true distribution $p_{data}$ must correspond to exactly one value of $\theta$. Otherwise, maximum likelihood can recover the correct $p_{data}$  but will not be able to determine which values of $\theta$ was used by the data generation process. 


For large $m$, consistent estimators have lower Mean Square Error (MSE) than the maximum likelihood estimator. For reasons of consistency and efficiency, maximum likelihood is often considered the preferred estimator to use for ML. When the number of examples is small enough to yield overfitting behavior, regularization strategies such as weight decay may be used to obtain a biased version of maximum likelihood that has less variance when training data is limited. 