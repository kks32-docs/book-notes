# Linear Algebra

> For more information refer to the [Matrix Cookbook](https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf)

## Types
A **Scalar** is a single number $s \in {\rm I\!R}$, when defining real-valued scalars 

A **vector** is an array of numbers. Intuitively we think of them as individual points in space. If a vector has $n$ elements and all the numbers are of type ${\rm I\!R}$ then the vector has a set of ${\rm I\!R}n$ elements written as ${\rm I\!R}^n$.

**Matrix** is a 2D array of numbers. A matrix $\textbf{A}$ with $m$ rows and $n$ columns is written as $\textbf{A} \in {\rm I\!R}^{m\times n}$. We identify elements in a matrix as $A_{i,j}$.

A **Tensor** is an array with more than two axis. $A_{i, j, k}$ is an example of a 3D tensor. 

## Operations and special types
*Linear combination* is a vector obtained by multiplying each vector $v^{(i)}$ by corresponding scalar coefficient $\sum_i c_i v^{(i)}$. A set of vectors is linearly independent if no vector in the set is a linear combination of other vectors.

A set of vectors is *linearly independent* if no vector in the set is a linear combination of other vectors. 

A *square* matrix that is *linearly dependent* is **singular**.

Two vectors are *orthogonal* if $x^T y = 0$ and have non-zero norms. If the norms of the vector are unity, then the orthogonal vectors are called *orthonormal*.

An *orthogonal matrix* is a square matrix whose rows are mutually orthonormal and whose columns are mutually orthonormal. Counterintuitively, the rows of the orthogonal matrix are not merely orthogonal but fully orthonormal.

## Linear systems.

A system of linear equations is written as:

$$ A x = b$$

where, $A \in {\rm I\!R}^{m \times n}$, $x \in {\rm I\!R}^n$ and $b \in {\rm I\!R}^m$ can be solved as $x = A^{-1} b$.  Once $A^{-1}$ is computed, it can be used to solve for different values of $b$.  $A^{-1}$ is primarily a theoretical tool and should not be used in practice. Since $A^{-1}$ can only be represented in computers with limited precision, algorithms which also use $b$ can provide an accurate estimation of $x$.  

For $A^{-1}$ to exist, $Ax = b$ must have exactly one solution. It is also possible for the system to have no solution or infinitely many solutions. In order for the system $Ax = b$ to have a solution for all values of $b \in {\rm I\!R}^m$, the column space of $\textbf{A}$ be all of ${\rm I\!R}^m$. If any value in ${\rm I\!R}^m$ is excluded from the column space, and then it is a potential value of $b$ that has no solution. The requirement of $\textbf{A} \in {\rm I\!R}^m$ implies $\textbf{A}$ has $m$ columns and $n \ge m$. Otherwise, the dimensionality of column space would be less than $m$. For example, consider a $3\times 2$ matrix. The target $b$ is 3-D, but $x$ is only 2-D, so modifying the value of $x$ at best enables us to trace output to a 2-D plane within ${\rm I\!R}^3$. The equation has a solution if and only if $b$ lies on that plane.

## Norm

Norm measures the size of a vector. More intuitively, norm of a vector is a measure of the distance from the origin to a point $x$. The norm is written as: $$||x||_p = \left(\sum_i |x_i|^p\right)^{1/p}$$

$L^2$ norm is the *Euclidean norm*, which is simply the Euclidean distance from the origin to point $x$. $L^2$ norm is used frequently in machine learning applications. However, in many applications, the squared $L^2$ norm is undesirable because it increases slowly closer to zero. 

In several ML applications, it is important to distinguish between elements that are exactly zero and those that are close to zero, in which case we often use the $L^1$ norm written as $||x||_1 = \sum_i |x_i|$. Every time an element moves away from 0 by $\varepsilon$, $L^1$ increases by $\varepsilon$. $L^1$ norm is often used as a substitute for counting the number of non-zero terms. 

It is sometimes essential to know the maximum magnitude in a vector and measured using the $L^\infty$ norm. 

Frobenius norm $||A||_F = \sqrt(\sum_{i,j} A_{i,j}^2)$ is the equivalent of $L^2$ norm for a matrix and is written as $\sqrt(Tr(AA^T))$.

## Determinant
The determinant is a mapping of a matrix to a scalar. The determinant is the product of all eigenvalues in the matrix. The absolute value of determinant can be thought of as how much the multiplication by the matrix expands or contracts the space. If the determinant is 0, then space is contracted completely along at least one of the dimensions, causing it to lose its volume. If the determinant is one, then the transformation preserves volume. 

## Eigendecomposition

Mathematical objects can be broken into constitutional parts that are universal and do not change based on the choice of representation. This allows us to observe the useful properties of the object. For example, an integer 12, irrespective of its choice of base (binary or decimal), can be decomposed as prime factors ($2 \times 2 \times 3$). These prime factors allow us to observe properties of the integer, such as it is not divisible by 5 or 7. 

Like the decomposition of an integer to its prime factors, a matrix can also be represented by its universal components called *eigenvalues* and *eigenvectors*. An eigenvector of a square matrix $\mathbf{A}$ is a non-zero vector $v$, such that multiplying by $\mathbf{A}$  only scales the value of $v$ by $\lambda$. 

$$ \mathbf{A} v = \lambda v$$

where, $v$ is the eigenvector and $\lambda$ is the eigenvalue. If $v$ is an eigenvector, so is any rescaled value $sv$ for $s \in {\rm I\!R}, s \ne 0$. Moreover, $sv$ has the same eigenvalues $\lambda$. So we are interested only in unit eigenvectors. Using the eigenvalues and eigenvectors we can reconstruct the original matrix $\mathbf{A}$ as $\mathbf{A}  = V diag(\lambda) V^{-1}$, where $V$ represents a collection of all eigenvectors represented as column vectors. 

Not all matrices can be decomposed, and some may have decomposition with complex numbers. However, all real symmetric matrix $\mathbf{A}$ is guaranteed to have _Eigendecomposition_.

If two or more eigenvectors share the same eigenvalue, then any set of orthogonal vectors lying in their space are also eigenvectors with that eigenvalue. We usually sort the entries of the eigenvalue matrix in descending order.

| eigenvalue properties | Matrix description    |
|------------------------|-----------------------|
| all values are zero        | Singular                       |
| all values are +ve         | Positive-definite          |
| +ve or zero                   | Positive-semidefinite  |
| all values are -ve          | Negative-definite        |
| -ve or zero                    | Negative-semidefinite |


## Singular Value Decomposition (SVD)
Every real matrix has a Singular Value Decomposition (SVD), but the same is not true for Eigendecomposition, for example, a non-square matrix. The SVD of matrix  $\mathbf{A}$  with size $m\times n$ is written as a product of 3 matrices all having sizes $m \times n$ as:  $\mathbf{A} = \mathbf{U}\mathbf{D}\mathbf{V}^T$, where $\mathbf{U}$ and $\mathbf{V}$ are orthogonal matrices, and $\mathbf{D}$ is a diagonal matrix, but may not be square. SVDs are useful in determining the pseudoinverse of matrices.

## Pseudoinverse

Consider a system of linear equations $\mathbf{A}x = y$, if matrix $\mathbf{A}$ is non-square then the left inverse of $\mathbf{A}$ is written as $x = \mathbf{B}y$. Depending on the problem, there may not be a unique mapping from $\mathbf{A}$ to $\mathbf{B}$. If $\mathbf{A}$ is taller than its wide, it is possible for this equation to have no solution (see [[#Linear systems]]). If $\mathbf{A}$ is wider than its tall, there could be multiple solutions. Moore-Penrose offers a pseudoinverse providing as many possible solutions when $\mathbf{A}$ is taller than its wide with minimal Euclidean norm $||x||_2$. For $\mathbf{A}$ wider than its tall it provides all possible $y$ in terms of the Euclidean norm $|| \mathbf{A} x - y ||_2$

## Principal Component Analysis (PCA)

Let's say we have $m$ points $x^1, \dots x^m$ in ${\rm I\!R}^n$, we want to apply lossy compression thereby storing these points using less memory, but we may lose some precision by doing this operation. Our goal is to lose as little precision as possible. The way to achieve this is to encode a lower dimension of the points $x^{(i)} \in {\rm I\!R}^n$ by finding a corresponding code vector $c^{(i)} \in {\rm I\!R}^m$, where $l < n$, we want to find some encoding function $f(x) = c$ and a decoding function $x \approx g(f(x))$. PCA is defined by our choice of decoding function, typically a matrix verse that maps the code vector back into ${\rm I\!R}^n$ space $g(c) = \mathbf{D}c$, where $\mathbf{D} \in {\rm I\!R}^{n\times l}$ computing the optimum code for the decoder may be a difficult problem. For easy encoding, PCA constructs the columns of $\mathbf{D}$ to be orthogonal to each other. However, this leads to many possible solutions. We can scale $\mathbf{D}_{:,i}$, so we constrain the columns of the matrix to be of unit norm. We need to find the code point $c^*$ for each input $x$, one way is to minimize the distance between $c^*$ and $x$ is to get the $L^2$ norm.

$$c^* = arg min_c || x - g(c) ||_2$$

We can optimize this function by taking its gradient, which yields $c = \mathbf{D}^T x$. To encode a vector we apply the encoder function $f(x) = \mathbf{D}^Tx$.  The PCA reconstruction operation involves $g(f(x)) = \mathbf{D} \mathbf{D}^T x$. Now we need to choose the encoding matrix $\mathbf{D}$ for which we use the Frobenius norm and optimize it for a given dimension.

$$\mathbf{D}^* = arg min_D \sqrt({\sum_{i,j} (x_j^{(i)} - r(x^{(i)}_j)^2})$$