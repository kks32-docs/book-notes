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

### Multivariate optimization
For functions with multiple inputs, we must take the __partial derivative__, $\frac{\partial f(x)}{\partial x}$, which measures how $f$ changes as only the variable $x_i$ increases at point $x$. The gradient of a function $f$ is the vector containing all the partial derivatives $\nabla_x f(x)$. 

__Directional derivative__ $f(x + \alpha \mathbf{u})$ is the slope of a function in the direction $\mathbf{u}$ (a unit vector) with respect to $\alpha$ at $\alpha = 0$. 

$$ \frac{\partial}{\partial \alpha} f(x + \alpha \mathbf{u}) = \mathbf{u}^T  \nabla_x f(x)$$

To minimize $f$, we like to find the direction in which $f$ decreases the fastest. 

$$min \mathbf{u}^T $\nabla_x f(x) = min ||u||_2 || \nabla_x f(x) ||_2 \cos \theta = min_u \cos \theta$$

Removing any terms independent of $\mathbf{u}$ and using its unit vector property, we get the minimization as a function of $\theta$ the direction between the gradient and $x$. This minimizes when $\mathbf{u}$ points in the opposite direction as the gradient. Gradient points uphill, negative of gradients points downhill; we can decrease $f$ by moving in the direction of negative gradient - this is called as the **method of steepest descent** or **gradient descent**.

A new point is determined as $x^\prime = x - \varepsilon \nabla_x f(x)$, where $\varepsilon$ is the __learning rate__ a positive scalar determining the size of the step. $\varepsilon$ is chosen to be a small value or we evaluate $f(x - \varepsilon \nabla_x f(x)$) for different values of $\varepsilon$ and choose a  $\varepsilon$ which yields the smallest objective function value - this method is called as the __line search__. Steepest descent converges when every element is zero (in practice almost close to zero). 

### Second derivatives

_Jacobian_ is a matrix of all partial derivatives of a function whose input and output both are vectors. The _second derivative_ tells how the slope (first derivative) changes as we vary the input. The second derivative intuitively is the __curvature__ of a function. The curvature tells us if a gradient step will cause as much of an improvement as we would expect. Consider a quadratic function:

- if the second derivative is zero, then there is no curvature - a flat line, the gradient of the line is 1, we can make a step size of $\varepsilon$ along the negative gradient then the function will decrease by $\varepsilon$, the gradient predicts the decrease correctly.
- If the second derivative is negative, the function curves down the cost function will decrease more than $\varepsilon$, faster than predicted by the gradient alone.
- If the second derivative is positive, the function curves upward, the cost function will decrease less than $\varepsilon$ (slower than expected by the gradient prediction and may actually begin to increase with larger step sizes). 

The Taylor Series approximation of the function $f(x)$ is:

$$f(x^{(0)} - \varepsilon \mathbf{g}) \approx f(x^{(0)}) - \varepsilon \mathbf{g}^T \mathbf{g} + 0.5 \varepsilon^2 \mathbf{g}^T \mathbf{H} \mathbf{g}$$

We must apply a correction to account for curvature in our optimization is $0.5 \varepsilon^2 \mathbf{g}^T \mathbf{H} \mathbf{g}$. Where, $\mathbf{H}$ _Hessian_ is the Jacobian of the gradient, $\mathbf{g}$ is the gradient. When this correction term is larger, then gradient descent may go uphill. When $\mathbf{g}^T \mathbf{H} \mathbf{g}$ is zero or positive, then $\varepsilon$ will forever decrease $f$. However, the Taylor series is unlikely to remain accurate for large values of $\varepsilon$. _The Hessian $\mathbf{H}$ controls the scale of the learning_. 


#### Second derivative test
The second derivative is useful in finding the local minimum or maximum. When the second derivative $f^{\prime\prime} (x) > 0$, the first derivative increases as we move to right and decreases as we move to left (like in a parabola $y = x^2$).

- $f^\prime(x) = 0$ and $f^{\prime\prime} (x) > 0$ is _local minima_
- $f^\prime(x) = 0$ and $f^{\prime\prime} (x) < 0$ is _local maxima_
- $f^{\prime\prime} (x) = 0$ test is inconclusive. 

For multi-dimensional problems, at critical point $\nabla_x f(x) = 0$, we examine the eigenvalues of the Hessian:
- if $\mathbf{H}$ is positive definite (all eigenvalues are positive) is a _local minimum_
- if $\mathbf{H}$ is negative definite is a _local maximum_
- When one or more eigenvalues are zero, then the test is inconclusive.

### Newton Method
In multi-dimensions, there is a different second derivative for each direction at a single point. The condition number of $\mathbf{H}$ means how much the second derivative differs from each other. When the condition number is poor (large value), the gradient descent performs poorly. This is because, in one direction, the derivative increases rapidly compared to the other and gradient descent is unaware of exploring the problem preferentially. The directional preference also affects the step size. The step size may be small enough to avoid overshooting, but maybe too small to make significant progress in the direction with less curvature. Using the information in Hessian could guide the optimization approach, and a common method using the Hessian is the __Newton Method__.

Consider a long elongated canyon with preferential slopes. Gradient descent would waste time repeatedly descending canyon walls because they are the steepest feature. The large positive eigenvalues of $\mathbf{H}$ corresponding to the eigenvector in the direction indicates that in this direction, the derivative is rapidly increasing. So the optimization based on the Hessian could predict the steepest direction is not actually a promising direction to explore in this context.

Newton's method is only useful when the nearby critical point is minima (all eigenvalues are positive definite for the Hessian), whereas gradient descent is not attracted to _saddle point_, unless the gradients point towards them. 

### Constraints and guarantees on optimization
In Machine Learning problems, due to the complex nature of the functions, there is no guaranteed solution. The closes to a guarantee are the *Lipschitz continuity* which dictates that a small change in input made by an algorithm such as gradient descent will have small changes in the output. 

Find a maximum or a minimum of a function $f(x)$ for $x \in \mathbb{S}$ is called as the constrained optimization. One common approach is to find a solution that is small in some sense, such as $||x|| < 1$. The gradient descent can be modified to solve constrained optimization by using a small step size that yields new points in $x$ that are feasible, i.e., they are part of the constraining subset.

