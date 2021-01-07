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