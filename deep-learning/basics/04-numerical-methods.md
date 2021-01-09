# Numerical Methods
Numerical methods refer to an iterative process of finding a solution rather than analytically deriving the symbolic notation of the correct answer. For example, optimization is a numerical method that refers to finding the value of an argument that minimizes or maximizes a function. 

## Underflow and overflow errors
Numerical methods are prone to errors. We need to represent infinitely many real numbers using a finite set of bit patterns, these result in representation errors. Hence, representing all real numbers involve some form of approximation. This rounding error is problematic. __Underflow__ occurs when rounding numbers near zero are rounded to zero. __Overflow__ is when a number with a large magnitude is approximated as $\pm \infty$.

__Softmax__ function is used to predict probabilities associated with multinoulli distributions:

$$softmax(x)_i = \frac{\exp(x_i)}{\sum_{j=1}^n \exp (x_j)}$$

If all $x_i$ are a constant $c$, then softmax is $1/n$.  However, when $c$ is a very large negative value, softmax will underflow, leading to division by zero. On the other hand, when $c$ is very large, it leads to overflow errors. 

## Poor conditioning
__Conditioning__ refers to how rapidly a function changes with respect to small changes in its input. Functions that change significantly when their inputs are perturbed cause major problems when the input values have rounding errors resulting in large changes in the output. Consider a function $f(x) = \mathbf{A}^{-1} x$ where $\mathbf{A} \in \mathbb{R}^{n \times n}$ has eigenvalue decomposition. The condition number is written as $max_{i,j} \left|\frac{\lambda_i}{\lambda_j}\right|$. The condition number refers to the ratio of the largest and the smallest eigenvalue. When the condition number is large, the matrix inversion is particularly sensitive to errors in the input. 

## Gradient-based optimization
Optimization refers to the maximization or minimization of a function by altering $x$. When a function is maximized, it is referred to as an __objective function__, and a function minimized is referred to as a __cost function__ or a __loss function__. The minimization operation is written as $x* = arg min f(x)$.

Derivative of a function $f(x)$ is written as $f^\prime (x)$ refers to the slope of a function $f$ at $x$. The derivative specifies how to scale a small change $\varepsilon$ in input to obtain the corresponding change in the output. 

$$f(x+\varepsilon) \approx f(x) + \varepsilon f^\prime(x)$$

Derivative is useful in minimizing because it tells us how to change $x$ to make a small improvement in $y = f(x)$. We know that $f(x - \varepsilon sign (f^\prime(x)))$ is less than $f(x)$ for small changes $\varepsilon$. We can reduce $f(x)$ by moving $x$ in small steps with opposite sign of the derivative - this technique is called as **gradient descent**.

Consider a function $f(x) = 0.5 x^2$ and its derivative is given as $f^\prime (x) = x$.  For $x < 0$, we have  $f^\prime (x)  < 0$, so we can decrease $f$ by moving rightward (increasing $x$). For $x > 0$, we have  $f^\prime (x) > 0$, so we can decrease $f$ by moving in the opposite direction to the derivative, i.e., moving leftward (decreasing $x$). The global minimum will be at $x = 0$. Since  $f^\prime (x) = 0$, gradient descent halts here. 

When  $f^\prime (x) = 0$ the derivative provides no information about which direction to move. Points where  $f^\prime (x) = 0$ are known as __critical points__ or __stationary points__. A __local minimum__ is a point where $f(x)$ is lower than at all neighboring points, so it is no longer possible to decrease $f(x)$ by making infinitesimal steps. A __local maximum__, on the other hand, is a point where $f(x)$ is higher than at all neighboring points, so it is not possible to increase $f(x)$ by making infinitesimal steps. Some points are neither maxima nor minima and are known as __saddle points__. A point that has the absolute lowest value of $f(x)$ is a __global minimum__.

There can be only one global minimum or multiple global minima of a function. In deep learning, we may optimize functions that may have many local minima that are not optimal and many saddle points surrounded by very flat regions, making optimization especially difficult when the input to the function is multidimensional. 