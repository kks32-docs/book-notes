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

## $L^1$ regularization
$L^1$ regularization is $\Omega(\theta) = || w||_1 = \sum_i |w_i|$ the sum of absolute values of individual parameters. The effect of $L^1$ regularization on the corresponding gradient of the objective function: $\nabla_w \hat{J}(w; X, y) = \alpha sign (w) + \nabla_w J(w; X, y)$. Unlike $L^2$ regularization, $L^1$ does not scale linearly with each $w$ but is a constant factor. In comparison to $L^2$ regulaizration, $L^1$ regularization results in a solution that is sparse, some parameters have an optimal value of zero. $L^2$ does not make parameter sparse while $L^1$ regularization does so far large values of $\alpha$. Sparsity property is used for features selection mechanism. When $L^1$ penalty causes a subset of weights to become zero, the corresponding features may safely be discarded. 

## Norm penalties as constrained optimization
The cost function regularized by a parameter norm penalty: $\hat{J}(\theta; X, y) = J(\theta; X, y) + \alpha \Omega(\theta)$. We think of parameter norm penalty as imposing a constraint on the weights. If $\Omega$ is the $L^2$ norm, then the weights are constrained to lie in an $L^2$ ball. If $\Omega$ is the $L^1$ norm, then the weights lie in a region limited $L^1$ norm. Usually, we do not know the size of the constraint region. Smaller $\alpha$ will result in a larger constraint region, while larger $\alpha$ will result in a smaller constraint region. 

Sometimes, we may use explicit constraints rather than penalities. Penalties can cause non-convex optimization to be stuck in local minima corresponding to small $\theta$. Explicit constraints do not encourage the weights to approach the origin and affects only when the weights become large and attempt to leave the constraint region. The explicit constraint also imposes stability on the optimization procedure, especially when using large learning rates. 

## Regularization and under-constrained optimization

Many linear models, including linear regression and PCA, depend on inverting $X^TX$. $X$ can become singular if $X$ has no variance in some direction or no variance is observed in some direction because of fewer examples (rows of $X$) than input features (cols of $X$). The regularized form $X^TX + \alpha I$ guarantees inversion. Most forms of regularization are able to guarantee convergence, e.g., weight decay makes the gradient descent quit increasing the magnitude of the weights when the slope of the likelihood is equal to the weight decay coefficients. 

## Data Augmentation

The best way to generalize an ML model is to train it on more data. One way to generate more data is to create fake data. A classifier needs to take a complicated, high dimensional input $x$ and summarize with a single category identity $y$. We can generate new $(x, y)$ pairs by transforming inputs $x$ in the training set. Data augmentation is particularly useful for object recognition problems. Translating an image by even a few pixels greatly improves generalization, even if the model is designed to be partially invariant to translations using convolution and polling techniques. Many operations, such as rotation and scaling of an image, are quite effective in generalization. However, one must be careful not to let transformations change the correct class, e.g., OCR of 'b' and 'd' or identifying numbers '6' and '9'. Data augmentation may show improved model performance, and the decrease in error is likely to be due to data augmentation rather than the ability of the model. 

Another form of data augmentation is injecting noise into the input. For many classifications and some regression, the task should still be possible to solve even if some random noise is added to the input. Nose injection also works when noise is added to the hidden unit. The addition of noise to input is equivalent to applying a penalty to the norm of weights. Adding noise to weights is a practical way to reflect uncertainty in the probability distribution of model weights. Noise applied to weights is equivalent to regulation, thus improving numerical stability by pushing models to regions where they are insensitive to small variations in weights. 

Most datasets have some mistakes in the $y$ labels. It can be harmful to maximize $\log p(y|x)$ when $y$ has errors. Label smoothing incorporates the small error with a probability of $1 - \epsilon$. 

## Multitask learning

Multitask learning is a way to improve generalization by pooling the examples. With more training data, the model generalizes. Similarly, sharing parts of models across tasks constraints the model towards good values (assuming sharing is justified) and yield better generalization.

