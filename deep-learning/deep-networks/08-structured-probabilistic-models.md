# Linear Factor Models
Many probability models have a latent variable $h$. $p_{model}(x) = \mathbb{E}_h p_{model}(x|h)$. We sample exploratory factors $h$ from a distribution as $h \sim p(h)$, where $p(h)$ is a factorial distribution $p(h) = \prod_i p(h_i)$ we then sample real-valued observable variables given the factors as: $x = Wh + b + noise$, where the noise is typically Gaussian. 

In factor analysis the latent variable $h$ is a unit variance of Gaussian $h \sim \mathcal{N}(h; 0, I)$, while the observable variable $x_i$ are conditionally independent given $h$. The noise is assumed to be drawn from diagonal covariance of the Gaussian distribution $(\psi = diag(\sigma^2))$. The role of latent variable is to capture the dependence between different observable variables $x_i$: $x = Wh + b + \sigma z$, which is the probabilistic PCA. When $\sigma \rightarrow 0$, probabilistic PCA becomes PCA. 

# Structured Probabilistic Model
A structure probabilistic model describes a probability distribution using a graph to describe which random variables in the probability distribution interact with each other directly. If we wish to model a distribution over a random vector $x$ containing $n$ discrete variables capable of taking on $k$ values each, then the naive approach of presenting $p(x)$ by storing it in a lookup table with one probability value per possible outcome requires $k^n$ parameters! A table-based approach suffers from memory, runtime, and statistical efficiency issues. In the real world, however, most variables influence each other only indirectly. 

Structure probabilistic models provide a framework for modeling direct interactions between random variables. This allows the model to have significantly fewer parameters and therefore be estimated reliably from fewer data. 

## Graphical models
Structure probabilistic model use graph to represent the interactions between random variables. Each node presents a random variable, each edge represents a direct interation. A directed graph from $a$ to $b$ defines a conditional probability distribution over $b$ via $a$. Local conditional probability distribution $p(x_i|P_\mathcal{G}(x_i))$, where $P_\mathcal{G}(x_i)$ is the of $x_i$ in $\mathcal{G}$. The probability distribution over $x$ is then defined as: $p(x) = \prod_i p(x_i | P_\mathcal{G}(x_i))$. For a relay race of three successive runner times in a team, the probability is defined as: $p(t_0, t_1, t_2) = p(t_0)p(t_1|t_0)p(t_2|t_1)$. In a graphical model the compute cost is $O(k^m)$, $m$ is the maximum number of variables appearing each side of the conditioning bar. As long as we can design a model such that $m << n$ (as long as it has fewer parents), we get dramatic advantages of graphical models.

An undirected graphical model is a structured probabilistic model that is defined on an undirected graph. For each clique, $\mathcal{C}$ (clique in a graph is a subset of nodes that are all connected to each other by an edge of the graph), a factor $\phi(\mathcal{C})$ measures the affinity of variables in the clique for being in each of their possible joint states. The factors are all nonnegative. The unnormalized probability distribution:

$$\hat{p}(x) = \prod_{c\in \mathcal{G}} \phi(\mathcal{c})$$

## Partition function
While unnormalize probability distribution is guranteed to be non-negative, it is not guranteed to sum or integrate to 1. To get normalized probability distribution (also called the Gibbs distribution):

$$p(x)= \frac{1}{z}\tilde{p}(x) \quad z = \int \tilde{p}(x) dx$$

Where $z$ is the value of the results in the probability distribution integrating to 1, and $z$ is the partial function. 

Directed models are defined directly in terms of the probability distribution from the start, while undirected models are defined loosely by the $\phi$ function that is then converted into a probability distribution. In an undirected graph, each variable dramatically affects the probability distribution level that a given set of $\phi$ functions correspond to. 

## Energy-based models
The main assumption in undirected probabilistic model is $\forall x, \tilde{p}(x) > 0$, we can enformce this constrain using energy-based models: $$\tilde{p}(x) = \exp(-E(x))$$. $E$ is the energy function, because $\exp(z)$ is positive for all $z$, this gurantees that no energy function will result in probaility of zero for any state of $x$. Any distribution of the above equation is called as the Boltzmann distribution. For this reason energy-based models are called Boltzmann machines. 

Cliques in an undirected graph correspond to factors of unnormalized probability functions. Because $\exp(a)\exp(b) =\exp(a+b)$, this means that different cliques in an undirected graph correspond to different terms of energy function. In other words, an energy-based model is a special kind of Markov network. Exponentials make each term in energy function a factor for different cliques. 

## Separation and d-separation

The edges represent direct interaction between variables. We want to know how variables interact indirectly. Some of the interactions can be enabled or disabled by observing other variables. Conditional independence in a graph is called *separation*. If two variables $a$ and $b$ are connected by a path involving only unobservable variables, then those variables are not separated. If no path exists between them or all paths contain an observable variable, they are separated. Paths involving only unobservable variables as "active" and path including an observable variable as "inactive." In a directed graph similar concept exists and is called *d-separation*, where $d$ means dependence. *A graph will never imply that independence exists when it does not, but may fail to encode an independence*.

## Sampling from graphical models
Graphical models facilitate the task of drawing samples from a model. Ancestral sampling with a directed graph produces a sample from join distribution represented by the model. Ancestral sampling is easy as long as it is easy to sample the conditional distribution $p(x_i | P_{\mathcal{G}}(x_i))$ but only applies to the directed graph. Another drawback is to sample a variable; we require all other variables that come earlier to be sampled in the ordered graph. 

Sampling from an undirected graph is an expensive multistep process. The conceptually simplest approach is *Gibbs sampling*. We iteratively visit each variable $x_i$ in an n-dimensional vector of random variables $x$, and draw a sample conditioned on all variables from $p(x_i | x_{-i})$. We need multiple passes and resample based o the updated values of the neighbors to have a fair sample from $p(x)$. It is hard to know when the sample has reached sufficient approximating the accuracy of desired distribution. 

Graphical representation allows explicit separation of knowledge from the inference of given existing knowledge. Graphical models dramatically reduce the cost of representing probability distribution and accelerate sampling. 

## Learning about Dependencies

A good generative model needs to accurately capture the distribution over the observed, or "visible," variables $v$. Often the diﬀerent elements of $v$ are highly dependent on each other. In the context of deep learning, the approach most commonly used to model these dependencies is to introduce several latent or "hidden" variables, $h$. The model can then capture dependencies between any pair of variables $v_i$ and $v_j$ indirectly, via direct dependencies between $v_i$ and $h$, and direct dependencies between $h$ and $v_j$. 

A good model of $v$, which did not contain any latent variables, would need to have very large numbers of parents per node in a Bayesian network or very large cliques in a Markov network. Just representing these higher-order interactions is costly—both in a computational sense, because the number of parameters that must be stored in memory scales exponentially with the number of members in a clique, but also in a statistical sense because this exponential number of parameters requires a wealth of data to estimate accurately.

## Inference and Approximate Inference
One of the main ways we can use a probabilistic model is to ask how variables are related to each other (e.g., given a set of medical tests, what disease is a patient might have). In the latent variable model, we want to extract the features $\mathbb{E}[h|v]$, describing the visible variable $v$. We train our models using the principle of maximum likelihood to compute $p(h|v)$. These are *inference problems*, which are intractable in deep learning even with structured graphical models.

## Deep learning in structured probabilistic models
DL does not always involve deep graphical models. In the graph, the depth of latent variable $h_i$ is the number of steps in the shortest path $j$ between $h_i$ and an observable variable. In DL, the latent variables do not take any semantic meaning and are free to invent the concepts needed to model a particular dataset. Hence, they are not easy for humans to interpret. On the other hand, latent variables on graphs have specific semantics in mind based on the graphical structure. 

Deep graph models typically have large groups of units connected to other groups of units. Traditional models have fewer connections between units to maintain the tractability of exact inference. When this constraint is very limiting, a popular approach is to use the *loopy belief algorithm*. Distributed representation usually yields dense graphs, and a large number of latent variables make it computationally very hard. 

## Restricted Boltzmann Machines (RBM)
RBM is an energy-based model with binary visible and hidden units. The energy function is: $E(v, h) = -b^T v -c^Th -v^TWh$. The variables $b$, $c$ and $W$ are unconstrained real-valued learnable parameters, $v$ and $h$ are visible and hidden units and their interaction is defined by matrix $W$. There is no direct interaction between any two hidden variables or any two visible units. Hence, "restricted" model of the general Boltzmann Machines. The RBM has nice properties. 
$p(h|v) = \prod_i p(h_i|v) \quad p(v|h) = \prod_i p(v_i|h)$. 

# Monte Carlo Methods
Randomized algorithms fall into two rough categories: Las Vegas algorithms and Monte Carlo algorithms. Las Vegas algorithms always return precisely the correct answer (or report that they failed). These algorithms consume a random amount of resources, usually memory or time. In contrast, Monte Carlo algorithms return answers with a random amount of error. The amount of error can typically be reduced by expending more resources (usually running time and memory). For any ﬁxed computational budget, a Monte Carlo algorithm can provide an approximate
answer.

Many problems in machine learning are so diﬃcult that we can never expect to obtain precise answers to them. This excludes precise deterministic algorithms and Las Vegas algorithms. Instead, we must use deterministic approximate algorithms or Monte Carlo approximations. Both approaches are ubiquitous in machine learning.

## Basics of Monte Carlo Sampling

When a sum or an integral cannot be computed exactly (for example, the sum has an exponential number of terms, and no exact simpliﬁcation is known), it is often possible to approximate it using Monte Carlo sampling. The idea is to view the sum or integral as if it were an expectation under some distribution and approximate the expectation by a corresponding average. Let

$$s = \sum_x p(x)f(x) = E_p[f(x)] \quad s = \int_x p(x)f(x)dx = E_p[f(x)]$$

be the sum(or integral) to estimate the constraint that p is a probability distribution(density) over random variable x. We can approximate $s$ by drawing $n$ samples from $x^{(1)}, \dots, x^{(n)}$ from $p$ then forming the empirical average:

$$\hat{s}_n = \frac{1}{n} \sum_{i=1}^n f(x^{(i)})$$

This approximation is justified as the estimator is unbiased, and the law of large numbers states that if the sample $x^{(i)}$ are i.i.d., then the average converges almost surely to the expected value: $\lim_{n\rightarrow\infty} \hat{s}_n=s$.

The *central limit theorem* tells us that the distribution of the average, $\hat{s}_n$, converges to a normal distribution with mean $s$ and variance $\frac{Var[f(x)]}{n}$.  This allows us to estimate conﬁdence intervals around the estimate $\hat{s}_n$, using the cumulative distribution of the normal density. A more general approach is to form a sequence of estimators that converge toward the distribution of interest. That is the approach of **Monte Carlo Markov chains**.

## Markov Chain Monte Carlo Methods

In many cases, we wish to use a Monte Carlo technique, but there is no tractable method for drawing exact samples from the distribution $p_{model}(x)$ or low variance importance sampling distribution $q(x)$. In the context of deep learning, this most often happens when $p_{model}(x)$ is represented by an undirected model. In these cases, we introduce a mathematical tool called a Markov chain to approximately sample from $p_{model}(x)$. The family of algorithms that use Markov chains to perform Monte Carlo estimates is called **Markov chain Monte Carlo methods (MCMC)**. The most standard, generic guarantees for MCMC techniques are only applicable when the model does not assign zero probability to any state. Therefore, it is most convenient to present these techniques as sampling from an energy-based model (EBM).


To understand why drawing samples from an energy-based model is diﬃcult, consider an EBM over just two variables, deﬁning a distribution $p(a, b)$. In order to sample $a$, we must draw $a$ from $p(a | b)$, and in order to sample $b$, we must draw it from $p(b|a)$. It seems to be an intractable chicken-and-egg problem. Directed models avoid this because their graph is directed and acyclic.

In an EBM, we can avoid this chicken-and-egg problem by sampling using a Markov chain. The core idea of a Markov chain is to have a state $x$ that begins with an arbitrary value. Over time, we randomly update $x$ repeatedly. Eventually $x$ becomes (very nearly) a fair sample from $p(x)$. Formally, a Markov chain is defined by a random state $x$ and a transition distribution $T(x^\prime | x)$ specifying the probability that a random update goes to state $x^\prime$ if it starts in-state $x$. Running the Markov chain means repeated updating the state $x$ to a value $x^\prime$ sampled from $T(x^\prime | x)$. 

Regardless of whether the state is continuous or discrete, all Markov chain methods consist of repeatedly applying stochastic updates until, eventually, the state begins to yield samples from the equilibrium distribution. Running the Markov chain until it reaches its equilibrium distribution is called **burning** in the Markov chain. After the chain has reached equilibrium, a sequence of inﬁnitely many samples may be drawn from the equilibrium distribution. They are identically distributed, but any two successive samples will be highly correlated with each other. A ﬁnite sequence of samples may thus not be very representative of the equilibrium distribution. One way to mitigate this problem is to return only every *n* successive sample so that our estimate of the equilibrium distribution statistics is not as biased by the correlation between an MCMC sample and the next several samples. Markov chains are thus expensive to use because of the time required to burn into the equilibrium distribution and the time required to transition from one sample to another reasonably decorrelated sample after reaching equilibrium. A commonly used number of Markov chains is 100.

Another diﬃculty is that we do not know in advance how many steps the Markov chain must run before reaching its equilibrium distribution. This length of time is called the **mixing time**. Testing whether a Markov chain has reached equilibrium is also diﬃcult. We do not have a precise enough theory to guide us in answering this question. Theory tells us that the chain will converge, but not much more. Instead, we run the Markov chain for an amount of time that we roughly estimate to be suﬃcient, and use heuristic methods to determine whether the chain has mixed.

## Gibbs sampling
So far we have described how to draw samples from a distribution
$q(x)$  by repeatedly updating $x \leftarrow x^\prime \sim T(x^\prime | x)$. We have not described how to ensure that $q(x)$ is a useful distirbution. In the context of deep learning, we commonly use Markov chains to draw samples from an energy-based model deﬁning a distribution $p_{model}(x)$. In this case, we want the $q(x)$ for the Markov chain to be $p_{model}(x)$. To obtain the desired $q(x)$, we must choose an appropriate $T(x^\prime | x)$. 

A conceptually simple and eﬀective approach to building a Markov chain that samples from $p_{model}(x)$ s to use **Gibbs sampling**, in which sampling from $T(x^\prime | x)$ is accomplished by selecting one variable $x_i$ and sampling it from $p_{model}$ conditioned on its neighbors in the undirected graph $\mathcal{G}$ defining the structure of the energy-based model. We can also sample several variables at the same time as long as they are conditionally independent, given all their neighbors. All the hidden units of an RBM may be sampled simultaneously because they are conditionally independent of each other, given all the visible units. Likewise, all the visible units may be sampled simultaneously because they are conditionally independent of each other, given all the hidden units. Gibbs sampling approaches that update many variables simultaneously in this way are called **block Gibbs sampling**.