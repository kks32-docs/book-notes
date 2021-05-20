# Tabular Deep Reinforcement Learning
## K-armed bandit
Each *k* actions has an expected or mean  reward given an action is selected; let's call this value of that action. An action selected on step *t* is $A_t$, and the corresponding reward is $R_t$. The value that an arbitrary action *a*, denoted as $q^*(a)$, is the expected reward given that $a$ is selected:

$$q^*(a) = \mathbb{E}[R_t | A_t = a]$$

We denote estimated value of an action *a* at time step *t* as $Q_t(a)$. We would like $Q_t(a)$ to be close to $q^*(a)$. 

If we maintain estimates of action values, then at any time step, there is at least one action whose estimated value is greatest. These are greedy actions, selecting these actions, we are *exploiting* the current knowledge. Instead, choosing a non-greedy action allows *exploring*, which improves the estimates of non-greedy actions. Whether to explore or exploit depends on the precise value of the estimates, uncertainties, and the number of remaining steps. 

## Action-Value Method
An action-value method estimates the values of actions for using the estimate to make an action selection decision.

$$Q_t(a) = \frac{\text{sum of rewards when a is taken prior to t}}{\text{number of times a is taken prior to t}} = \frac{\sum_{i=1}^t R_i \cdot \mathbb{1}_{A_i = a}}{\sum_{i=1}^t \mathbb{1}_{A_i = a}}$$

$\mathbb{1}_{predicate}$ is a random variable whose value is one if the predicate is true and 0 if not. An initial default value of $Q_t(a)$ of 0 is used when there are no prior steps. 

The selection of greedy action $A_t = \arg\max_a Q_t(a)$. Greedy action selection is always exploiting the current knowledge to maximize the immediate reward. A simple alternative is to behave greedily, most of the time, except for every once in a while, say a small probability of $\varepsilon$ act randomly. These actions are *near-greedy* or $\varepsilon-$greedy actions.