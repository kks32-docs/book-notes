# Regularization
Regularization is any modification we make to a learning algorithm intended to reduce its generalization error but not its training error. Regularization is a trade-off between increasing bias for a reduced variance. Many regularization approaches are based on limiting the capacity of models by adding a penalty $\Omega(\theta)$ to the objective function $J$. The regularized objective function: $\hat{J}(\theta; X, y) = J(\theta, X, y) + \alpha \Omega(\theta)$, where $\alpha \in [0, \infty]$ is a hyperparameter that weights the relative contribution of $\Omega$ with respect to the objective function $J$. When $\alpha = 0$ means no regularization. $\Omega$ only penalizes the weights at each layer and leaves bias unregularized as each bias only controls one variable and does not induce too much variance. Also, regularization bias can introduce too much underfitting. 

## $L^2$ regularization

$L^2$ regularization of objective function is: $$\Omega(\theta) = 0.5 ||w||_2^2$$. Assuming no bias parameter, $\theta$ is $w$:

$$hat{J}(\theta; X, y) = J(\theta, X, y) + \frac{\alpha}{2} w^T w$$

The gradient: 
$$\nabla_w hat{J}(\theta; X, y) = \nabla_w J(\theta, X, y) + \alpha w$$

At each gradient step to update weight:
$$ w \leftarrow (1 - \varepsilon \alpha) w - \varepsilon (\nabla_w J (w; X, y))$$

The addition of weight decay has modified the learning to rule to multiplicatively shrink the weight vector by a constant factor on each step before performing the usual gradient update. 

The effect of weight decay is to rescale the unregularized training cost $w^* = \arg \min_w J(w)$ along the axes defined by the eigen vectors of $H$, 
$$hat{J}(\theta) = J(w^*) + \frac{1}{2} (w - w^*)^T H(w - w^*)$$
where $H$ is the Hessian of $J$. Specifically the component of $w^*$ that is aligned with the i-th eigen vector of H is rescaled by a factor $\lambda_i / (\lambda_i + \alpha)$. Along the direction where the eigen values of $H$ are large $\lambda_i >> \alpha$, the effect of regularization is relatively small. Yet components with $\lambda_i << \alpha$ will be shrunk to nearly zero magnitude. Components of weight vectors corresponding to unimportant directions are decayed away with the use of regularization. 

In $L^2$ regularization of linear regression, the cost function: $(Xw - y)^T (Xw -y)$ with an objective function is: $(Xw - y)^T (Xw -y) + \frac{1}{2} \alpha w^T w$. The normal equations changes from $w = (X^TX)^{-1}X^Ty$ to  $w = (X^TX + \alpha I)^{-1}X^Ty$. $X^TX$ is proportional to the covariance matrix and $L^2$ adds an additional $\alpha$ to the diagonal. The diagonals correspond to the variance of each input feature. $L^2$ causes learning algorithm to "perceive" $X$ to have high variance, thus shrinking weights of features where covariance with the output target is low compared to this added variance.
