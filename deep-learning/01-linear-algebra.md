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
