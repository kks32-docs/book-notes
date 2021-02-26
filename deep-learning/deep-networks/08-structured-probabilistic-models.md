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