# Information theory
Information theory quantifies the amount of uncertainty in a probability distribution. Information theory specifies how much information is present in a signal and is used to characterize probability distribution or quantify the similarity between probability distributions. Learning an unlikely event has occurred is more informative than a likely one (e.g., knowing if a solar eclipse occurred is more informative than the sun rose in the morning). We like to quantify information such that:
- Likely information or events that are guaranteed to occur have low or no information content.
- Less likely events have higher information content
- Independent events should have additive information content. 

## Shannon entropy

*Self information * of an event $X = x$ is written as $I(x) = - \log P(x)$ and has a unit of _nats_. One _nat_ is the amount of information gained by observing an event with $1/e$ probability. This uses a natural log with base $e$. Instead, if we use base two, then _nat_ is a scaled unit referred to as _bits_ or _shannons_. When $x$ is continuous, events with unit density still have zero information, despite not being an event that is guaranteed to occur. 

Self-information deals with a single outcome. The amount of uncertainty in the entire probability distribution function is given by **Shannon entropy** (discrete values) or differential entropy (when $x$ is continuous):

$$H(x) = \mathbb{E}_{x\approx P} [I(x)] = -\mathbb{E}_{x\approx P} [\log P(x)]$$

Shannon entropy is the expected amount of information in an event drawn from the distribution and acts as a lower bound on the number of bits needed on average to encode symbols drawn from a distribution $P$. Distributions that are deterministic have a low entropy, and distributions that are uniform have high entropy.  

## Kullback-Leibler (KL) divergence
If we have two distributions, $P(x)$ and $Q(x)$, we can measure how different these two distributions are using the Kullback-Leibler KL divergence:
$$ D_{(KL)} (P || Q) = \mathbb{E}_{x\approx P} [\log P(x) - \log Q(x)]$$

The KL divergence is zero if $P$ and $Q$ have the same distribution (discrete values) or equal "almost everywhere" (continuous). KL is always non-negative and can be thought of as a measure of the distance between 2 distribution. However, $D_{(KL)} (P || Q) \ne D_{(KL)} (Q || P)$. The choice of use between the two depends on which approximation to use. If we want to place high probability anywhere, the true distribution places a high probability, or we rarely place high probability when the true distribution places low probability.

*Cross entropy* is equivalent to minimizing the KL divergence.

## Structure probability model
A structured probability model refers to the factorization of a probability distribution on a graph. Instead of writing a probability distribution function (PDF) on a single function, we split a PDF into many factors that we can multiply together and also record their dependence. 

The factorization of a PDF can be represented as a graph $\mathcal{G}$ with nodes and edges. Each node in a graph represents a random variable, and the edge connecting the nodes represent the probability distribution of the direct interactions between the two random variables in a directed graph. Graphical models allow us to quickly see properties of distributions like if two random variables interact directly or indirectly through another variable.