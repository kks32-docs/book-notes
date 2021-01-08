# Probability and Information Theory
Probability theory is a mathematical framework to quantify uncertainty as well as axioms for deriving new uncertainty statements. Probability can be seen as an extension of logic. Logic provides formal rules for determining true or false propositions, and probability provides formal rules for determining the likelihood of the proposition being true, given the likelihood of other propositions. Information theory quantifies the amount of uncertainty in a probability distribution. Machine learning deals with uncertain quantities and sometimes stochastic (non-deterministic) quantities. The sources of uncertainties are:
- _Inherent stochastic_ nature of the system (e.g., random events or quantum world)
- _incomplete observability_ of the system. The system may be deterministic; however, we cannot observe all quantities (e.g., Monty Hall problem from the player point of view)
- _Incomplete model_: Although the system can be observed in totality, the approximation of the real world leads to disregarding some information (e.g., discretization of space-time)

Due to these uncertainties, in many cases, it is more practical to use a simple but uncertain rule rather than a complex but certain one. Hence we need the means to represent and reason about uncertainty. Probability theory is used in ML for two reasons: 
- How ML should reason, we build models to compute and approximate the real world
- Offers a theoretical framework to analyze the behavior of ML models.


## Kinds of probability
There are two kinds of probability: (i) **Frequentist probability** - analyze the frequency of events (e.g., rolling a certain number on a dice throw), if the outcome has a probability _p_ of occurring, it means that if the event is repeated infinite times then the proportion _p_ of the repetitions would result in that outcome, and (ii) **Bayes probability** - analyze qualitative levels of uncertainty and represent the degree of belief with 1 indicating absolute certainty of an event occurring and 0 representing the absolute certainty of non-occurrence of an event (e.g., diagnosis of a patient to have a certain disease). A small set of common-sense assumptions implies that the same axioms must control both kinds of probabilities. 


## Probability distribution
A random variable represents different states that are possible. It can be discrete (finite of countably infinite states) or continuous variable (associated with a real value).

#pmf #probability_mass_function
The probability distribution is how likely a random variable or a set of random variables is to take each of its possible states. **Probability Mass Function (PMF)** is the probability distribution over discrete variables and is written as $P(x)$, the probability that $X = x$ with 1 being certain and 0 is impossible. This is also written as $P(X=x)$ or $X ~ P(x)$.

If PMF can act on many variables at a time it is called _joint probability_ $P(X=x, Y=y)$ and must satisfy the following conditions:
- $P$ has all possible states of $x$
- $\forall x \in X, \quad 0 \le P(x) \le 1$
- $\sum_{x \in X} P(x) =1$, this _normalized_ condition ensures that the probabilities are not greater than one.


A _uniform distribution_ represents all possible states equally. If there are $k$ states for a variable $x$ then $P(X=x_i) = \frac{1}{k}$.

**Probability Density Function (PDF)** represents the probability distribution over continuous variables and is written as $p(x)$. $p(x)$ does not give a probability of a specific state instead of the probability of landing in an infinitesimal region of volume $\delta x$ by $p(x)\delta x$ and must satisfy the following conditions:
- $p$ has all possible states of $x$
- $\forall x \in X, \quad p(x) \ge 0$ we do not require $p(x) \le 1$
- $\int p(x)dx =1$.

Sometimes we know the PDF over a set of variables and we are interested to know the probability distribution over a subset of them - _Marginal Probability Distribution_. We can compute $p(x)$ if we know $p(x, y)$ as $\forall x \in X P(X=x) = \sum_y P(X=x, Y=y)$.

## Conditional probability
In many cases, we are interested in knowing the probability of some event $y$, given that some other event $(x)$ has happened:

$$P(Y=y | X=x) = \frac{P(Y=y, X=x)}{P(X=x)}$$

This is only valid when $P(X=x) > 0$. It is important not to confuse conditional probability with computing what might happen if some action were undertaking. Determining a consequence of an action is called _inversion query_ in the domain of _causal modeling_.

### Chain rule
[[https://en.wikipedia.org/wiki/Chain_rule_%28probability%29]]
The chain rule for two random events $A$ and $B$  says

$P(A, B) = P(B|A)\cdot P(A)$ 

This rule is illustrated in the following example. Urn 1 has 1 black ball and 2 white balls and Urn 2 has 1 black ball and 3 white balls. Suppose we pick an urn at random and then select a ball from that urn. Let event $A$ be choosing the first urn: $P(A) = 0.5$. Let event $B$ be the chance we choose a white ball. The chance of choosing a white ball, given we have chosen the first urn, is $P(B|A) = 2/3$. Even $A \cap B$ is their intersection, choosing the first urn and a white ball from it. The chain rule for probability can find the probability of $P(A,B)$:

$$P(A, B) = P(B|A) \cdot P(A) = 2/3 \times 1/2 = 1/3$$

## Independent variables
Two variables $x$ and $y$ are independent ($x \perp y$) if the probability distribution can be expressed as a product of 2 factors, independent of each other. 

$$ P(X=x, Y=y) = p(X=x) * p(Y=y)$$

## Expected value, variance, and covariance
**Expected value** of a function $f(x)$ with respect to a probability function $P(x)$ is the mean value that $f$ takes when $x$ is drawn from $P(x)$. 
$$\mathbf{E}_{x}~P[f(x)] = \sum_x P(x) f(x)$$ 
For continuous variables
$$=\int p(x) f(x) dx$$

**Variance** gives how much a value of a function of a random variable $x$ vary as we sample different values of $x$ from its probability distribution.
$$Var(f(x)) = \mathbf{E}[(f(x) - \mathbf{E}[f(x))^2]$$
when the variance is low, the values of $f(x)$ clusters around their expected value. The square root of variance is defined as the **standard deviation**.

**Covariance** gives how much two values are linearly related to each other as well as the scale of these variables.

$$Cov(f(x), g(y)) = \mathbf{E}[(f(x) - \mathbf{E}[f(x)])(g(y) - \mathbf{E}[g(y)])]$$

High values of covariance indicate that the values change very much and are far from their respective means. A high positive value means both values change significantly and simultaneously. A negative value means one variable has a high value, and the other takes a relatively low value. Two variables that are independent have zero covariance. This is a necessary but not a sufficient condition, and independence has a stronger requirement. The covariance of a random vector $x$ is a $n\times n$ matrix: $Cov(x)_{i,j} = Cov(x_i, x_j)$. 

## Distributions
### Bernouilli distribtion
Distribution over a set of single binary random variable controlled by a single parameter $\Phi \in [0, 1]$, which gives the probability of the random variable is equal to 1. 
$$ P(X =1) = \Phi \quad P(X=0) = 1 - \Phi$$

### Multnouilli distribution
Distribution over a single discrete variable with $k$ different states (a finite set of possibilities for $k$) is often used to refer to distribution over categories of objects. 

### Normal or Gaussian distribution
Distribution over real numbers defined by two parameters: $\mu \in \mathbb{R}$ defines the coordinates of the central peak and is $\mathbb{E}[x] = \mu$ and the standard deviation $\sigma(0, \infty)$. The normal distribution is a sensible choice in the absence of prior knowledge to represent real numbers because of (i) **central limit theorem**, the sum of many independent random variables is approximately normally distributed, and (ii) normal distribution encodes the maximum amount of uncertainty over real numbers. The normal distribution inserts the least amount of prior knowledge into the model. 

### Other distributions
The *exponential distribution* is useful in ML when we want a distribution with a sharp point at $x=0$. The probability is zero for all negative values of $x$.

The *Laplace distribution* allows placing a sharp peak of probability mass function at an arbitrary point $\mu$. 

*Dirac function* $p(x) = \delta (x - \mu)$ is defined such that it is zero-valued everywhere except at zero and yet it integrates to 1. Dirac distribution function is useful to define the empirical distribution. The *empirical distribution* function estimates the cumulative distribution function that generated the points in the sample.

*Mixture distributions* are used to construct more complex distributions from simpler ones. *Gaussian mixture models* are written as $p(x | c=i)$. The _prior probability_ $\alpha_i = P(c=i)$ to each component $i$ is the model's belief about $c$ before observing $x$. In comparison, $p(c|x) is the _posterior probability_ as its computed after the observation of $x$. 

## Logistic sigmoid
$$ \sigma (x) = \frac{1}{1+\exp(-x)|}$$
Logistic sigmoid is used to produce $\Phi$ in the Bernouilli distribution as it ranges from $[0, 1]$. The sigmoid function saturates for very positive value of negative values (i.e., becomes flat) and is insensitvie to input. 
The function $\sigma^{-1}(x)$ is the logit function in statistics.

## Softplus function
Softplus function helps define $\beta$ or $\sigma$ parameters of normal distribution.
$$\zeta (x) = \log(1+\exp(x))$$ 
The softplus function ranges from 0 to $\infty$, the function is "_softer_" version of $x^+ = max(0, x)$.

## Bayes rule
We know $P(y|x)$ to compute the conditional probability $P(x|y)$ we invoke the Bayes rule. 
$$P(x|y) = \frac{P(x)\cdot P(y|x)}{P(y)}$$
The knowledge of $P(y)$ is not needed and can be derived as $\sum_x P(y|x)\cdot P(x)$.
