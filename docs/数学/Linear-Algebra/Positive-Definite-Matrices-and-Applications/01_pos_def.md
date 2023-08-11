# Symmetric Matrices and Positive definiteness

Symmetric metrices are good, their eigenvalues are real and each has a complete set of orthonormal eigenvectors. Positive definite matrices are even better.

## Symmetric Matrices

A `symmetric matrices` is one for which $A = A^T$. If a matrix has some special property (e.g. it's a Markov matrix), its eigenvalues and eigenvectors are likely to have special properties as well. For a symmetric matrix with real number entries, the  eigenvalues are real numbers and it's possible to choose a complete set of eigenvectors that are perpendicular (or even orthonormal).

If A has n independent eigenvectors we dan write $A = S \Lambda S^{-1}$. If A is symmetric we can write $A = Q \Lambda Q^{-1} = Q \Lambda Q^T$, where Q is an orthogonal matrix. Mathematicians call this the `spectral theorem` and think of the eigenvalues are the `spectrum` of the matrix. In mechanics it's called the `principal axis theorem`.

In addition, any matrix of the form $Q \Lambda Q^T$ will be symmetric.

## Real Eigenvalues

Why are the eigenvalues of symmetric matrix real? Suppose A is sysmmetric and $A\mathbf{x} = \lambda \mathbf{x}$. Then we can conjugate to get $\bar{A}\bar{\mathbf{x}} = \bar{\lambda} \bar{\mathbf{x}}$. If the entries of A are real, this becomes $A \bar{\mathbf{x}} = \bar{\lambda} \bar{\mathbf{x}}$. (This proves that complex eigenvalues of real valued matrices come in conjugate pairs.)

Now transpose to get $\bar{\mathbf{x}}^T A^T = \bar{\mathbf{x}}^T \bar{\lambda}$. Because A is symmetric we now have $\bar{\mathbf{x}}^T A = \bar{\mathbf{x}}^T \bar{\lambda}$. Multiplying both sides of this equation on the right by $\mathbf{x}$ gives:

$$
\bar{\mathbf{x}}^T A \mathbf{x} = \bar{\mathbf{x}}^T \bar{\lambda} \mathbf{x}
$$

On the other hand, we can multiply $A\mathbf{x} = \lambda \mathbf{x}$ on the left by $\bar{\mathbf{x}}^T$ to get:

$$
\bar{\mathbf{x}}^T A \mathbf{x} = \bar{\mathbf{x}}^T \lambda \mathbf{x}
$$

Comparing the two equations we see that $\bar{\mathbf{x}}^T \bar{\lambda} \mathbf{x} = \bar{\mathbf{x}}^T \lambda \mathbf{x}$ and, unless $\bar{\mathbf{x}}^T \mathbf{x}$ is zero, we can conclude $\lambda = \bar{\lambda}$ is real.

How do we know $\bar{\mathbf{x}}^T\mathbf{x} \ne 0$?

$$
\bar{\mathbf{x}}^T \mathbf{x} = \begin{bmatrix} \bar{x_1} & \bar{x_2} & \cdots & \bar{x_N} \end{bmatrix} \begin{bmatrix} x_1 \\ x_2 \\ \vdots \\ x_n \end{bmatrix} = |x_1|^2 + |x_2|^2 + \cdots + |x_n|^2
$$

If $\mathbf{x} \ne 0$ then $\bar{\mathbf{x}}^T \mathbf{x} \ne 0$.

With complex vectors, as with complex numbers, multiplying by the conjugate is often helpful.

Symmetric matrices with real entries have $A = A^T$, real eigenvalues, and perpendicular eigenvectors. If A has complex entries, then it will have real eigenvalues and perpendicular eigenvectors if and only if $A = \bar{A}^T$. (The proof of this follows the same pattern.)

## Projection onto Eigenvectors

If $A = A^T$, we can write:

$$
\begin{align}
A &= Q \Lambda Q^T \\
  &= \begin{bmatrix} \mathbf{q}_ 1 & \mathbf{q}_ 2 & \cdots & \mathbf{q}_ n \end{bmatrix} \begin{bmatrix} \lambda_1 & 0 & \cdots & 0 \\ 0 & \lambda_2 & \cdots & 0 \\ \vdots & \vdots & \ddots & vdots \\ 0 & 0 & \cdots & \lambda_n \end{bmatrix} \begin{bmatrix} \mathbf{q}_ 1^T \\ \mathbf{q}_ 2^T \\ \vdots \\ \mathbf{q}_ n ^T\end{bmatrix}
  &= \lambda_1 \mathbf{q}_ 1 \mathbf{q}_ 1^T + \lambda_2 \mathbf{q}_ 2 \mathbf{q}_ 2^T + \cdots + \lambda_n \mathbf{q}_ n \mathbf{q}_ n^T
\end{align}
$$

The matrix $\mathbf{q}_ k \mathbf{q}_ k^T$ is the projection matrix onto $\mathbf{q}_ k$, so every symmetric matrix is a combination of perpendicular projection matrices.

## Information about Eigenvalues

If we know that eigenvalues are real, we can ask whether they are positive or negative. (Remember that the signs of the eigenvalues are important in solving systems of differential equations).

For every large matrices A, it's impractical to compute eigenvalues by solving $|A - \lambda I| = 0$. However, it's not hard to compute the pivots, and the signs of the pivots of a symmetric matrix are the same as the signs of the eigenvalues:

$$
\text{number of positive pivots} = \text{number of positive eigenvalues}
$$

Because the eigenvalues of $A + bI$ are just b more than the eigenvalues of A, we can use this fact to find which eigenvalues of a symmetric matrix are greater or less than any real number b. This tells us a lot about the eigenvalues of A even if we can't compute them directly.

## Positive Definite Matrices

A `positive definite matrix` is a symmetric matrix A for which all eigenvalues are positive. A good way to tell if a matrix is positive definite is to check that all its pivots are positive.

Let:

$$
A = \begin{bmatrix} 5 & 2 \\ 2 & 3 \end{bmatrix}
$$

The pivots of this matrix are $5$ and $(det A) / 5 = 11 / 5$. The matrix is symmetric and its pivots (and therefore eigenvalues) are positive, so A is a positive definite matrix. Its eigenvalues are the solution to:

$$
|A - \lambda I| = \lambda^2 - 8\lambda + 11 = 0
$$

i.e. $4 \pm \sqrt{5}$.

The determinant of a positive definite matrix is always positive but the determinant of $\begin{bmatrix} -1 & 0 \\ 0 & -3 \end{bmatrix}$ is also positive, and that matrix isn't positive definite. If all of the subdeterminants of A are positive (determinants of the k by k matrix in the upper left corner of A, where $1 \le k \le n$), then A is positive definite.

The subject of positive definite matrices brings together what we've learned about pivots, determinants and eigenvalues of square matrices. Soon we'll have a chance to bring together what we've learned in this course and apply it to non-square matrices.
