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