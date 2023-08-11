# Diagonalization and Powers of A

We know how to find eigenvalues and eigen vectors. In this lecture we learn to `diagonalize` any matrix that has $n$ independent eigenvectors and see how diagonalization simplifies calculations. The lecture concludes by using eigenvalues and eigenvectors to solve `difference equations`.

## Diagonalizing a Matrix S^-1AS=Lambda

If A has n linearly `independent eigenvectors`, we can put those vectors in the columns of a matrix S(square, invertible), then:

$$
\begin{align}
AS &= A \begin{bmatrix} \mathbf{x_1} & \mathbf{x_2} & \cdots & \mathbf{x_n} \end{bmatrix} \\
   &= \begin{bmatrix} \lambda_1 \mathbf{x_1} & \lambda_2 \mathbf{x_2} & \cdots & \lambda_n \mathbf{x_n} \end{bmatrix} \\
   &= S \begin{bmatrix} \lambda_1 & 0 & \cdots & 0 \\ 0 & \lambda_2 & \cdots & 0 \\ \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & \cdots & \lambda_n \end{bmatrix} \\
   &= S\Lambda
\end{align}
$$

Note that $\Lambda$ is a diagonal matrix whose non-zero entries are the eigenvalues of $A$. Because the columns of $S$ are independent, $S^{-1}$ exists and we can multiply both sides of $AS = S \Lambda$ by $S^{-1}$:

$$
S^{-1} AS = \Lambda
$$

Equivalently,

$$
A = S \Lambda S^{-1}
$$

## Powers of A

What are the eigenvalues and eigenvectors of $A^2$?

$$
\begin{align}
\text{If } A\mathbf{x} &= \lambda \mathbf{x} \\
\text{then } A^2 \mathbf{x} &= \lambda A \mathbf{x} = \lambda^2 \mathbf{x} \\
\end{align}
$$

The eigenvalues of $A^2$ are the squares of the eigenvalues of $A$. The eigenvectors of $A^2$ are the same as the eigenvectors of $A$. If we write $A = S \Lambda S^{-1}$ then:

$$
A^2 = S \Lambda S^{-1} S \Lambda S^{-1} = S \Lambda^2 S^{-1}
$$

Similarly:

$$
A^k = S \Lambda^k S^{-1}
$$

tells us that raising the eigenvalues of $A$ to the kth power gives us the eigenvalues of $A^k$, and that the eigenvectors of $A^k$ are the same as those of A.

Theorem:

> If A has n independent eigenvectors with eigenvalues $\lambda_i$, then $A^{k} \to 0$ as $k \to \infty$ if and only if all $|\lambda_i| < 1$.

A is guaranteed to have n independent eigenvectors (and be `diagonalizable`) if all its eigenvalues are different. Most matrices do have distinct eigenvalues.

## Repeated Eigenvalues

If A has repeated eigenvalues, it may or may not have n independent eigenvectors. For example, the eigenvalues of the identity matrix are all 1, but that matrix still has n independent eigenvectors.

If A is the triangular matrix $\begin{bmatrix} 2 & 1 \\ 0 & 2 \end{bmatrix}$ its eigenvalues are 2 and 2. Its eigenvectors are in the nullspace of $A - \lambda I = \begin{bmatrix} 0 & 1 \\ 0 & 0 \end{bmatrix}$ which is spanned by $\mathbf{x} = \begin{bmatrix} 1 \\ 0\end{bmatrix}$. This particular A does not have two independent eigenvectors.

## Difference Equation u_{k + 1} = Au_{k}

Start with a given vector $\mathbf{u_0}$. We can create a sequence of vectors in which each new vector is A times the previous vector:

$$
\mathbf{u_{k + 1}} = A \mathbf{u_{k}}.
$$

$\mathbf{u_{k + 1}} = A \mathbf{u_k}$ is a `first order difference equation`, and $\mathbf{u_k} = A^k \mathbf{u_0}$ is a solution to this system.

We get a more statisfying solution if we write $\mathbf{u_0}$ as a combination of eigenvectors of A:

$$
\mathbf{u_0} = c_1 \mathbf{x_1} + c_2 \mathbf{x_2} + \cdots + c_n \mathbf{x_n} = S \mathbf{c}
$$

Then:

$$
A \mathbf{u_0} = c_1 \lambda_1 \mathbf{x_1} + c_2 \lambda_2 \mathbf{x_2} + \cdots + c_n \lambda_n \mathbf{x_n} = \Lambda S \mathbf{c}
$$

and:

$$
\mathbf{u_k} = A^k \mathbf{u_0} = c_1 \lambda_1^k \mathbf{x_1} + c_2 \lambda_2^k \mathbf{x_2} + \cdots + c_n \lambda_n^k \mathbf{x_n} = \Lambda^k S \mathbf{c}
$$

## Fibonacci Sequence

The Fibonacci sequence is $0, 1, 1, 2, 3, 5, 8, 13, \cdots$. In general, $F_{k + 2} = F_{k + 1} + F_{k}$. If we could understand this in terms of matrices, the eigenvalues of the matrices would tell us how fast the numbers in the sequence are increasing.

$\mathbf{u_{k + 1}} = A \mathbf{u_{k}}$ was a first order system. $F_{k + 2} = F_{k + 1} + F_{k}$ is a `second order scalar equation`, but we can convert it to first order linear system by using a clever trick.

If $\mathbf{u_k} = \begin{bmatrix} F_{k + 1} \\ F_{k}\end{bmatrix}$, then:

$$
\begin{align}
F_{k + 2} &= F_{k + 1} + F_{k} \\
F_{k + 1} &= F_{k + 1}
\end{align}
$$

is equivalent to the first order system $\mathbf{u_{k + 1}} = \begin{bmatrix} 1 & 1 \\ 1 & 0 \end{bmatrix} \mathbf{u_{k}}$.

What are the eigenvalues and eigenvectors of $A = \begin{bmatrix} 1 & 1 \\ 1 & 0 \end{bmatrix}$? Because A is systemmetric, its eigenvalues will be real and its eigenvectors will be orthogonal.

Because A is a 2 by 2 matrix we know its eigenvalues sum to 1(the trace), and their product is $-1$(the determinant).

$$
\begin{vmatrix}
A - \lambda I
\end{vmatrix}
=
\begin{vmatrix}
1 - \lambda & 1 \\
1 & -\lambda \\
\end{vmatrix}
= \lambda^2 - \lambda - 1
$$

Setting this to zero we find $\lambda = \frac{1 \pm \sqrt{1 + 4}}{2}$; i.e. $\lambda_1 = \frac{1}{2} (1 + \sqrt{5}) \approx 1.618$ and $\lambda_2 = \frac{1}{2} (1 - \sqrt{5}) \approx -0.618$. The growth rate of the $F_k$ is controlled by $\lambda_1$, the only eigenvalue with absolute value greater than 1. This tells us that for large k, $F_{k} \approx c_1(\frac{1 + \sqrt{5}}{2})^k$ for some constant $c_1$. (Remember $\mathbf{u_k} = A^k \mathbf{u_0} = c_1 \lambda_1^k x_1 + c_2 \lambda_2^k x_2$, and here $\lambda_2^k$ goes to zero since $|\lambda_2| < 1$)

To find the eigenvectors of A note that:

$$
(A - \lambda I) \mathbf{x} = \begin{bmatrix} 1 - \lambda & 1 \\ 1 & -\lambda \end{bmatrix} \mathbf{x} = 0
$$

when $\mathbf{x} = \begin{bmatrix} \lambda \\ 1 \end{bmatrix}$, so $\mathbf{x_1} = \begin{bmatrix} \lambda_1 \\ 1 \end{bmatrix}$ and $\mathbf{x_2} = \begin{bmatrix} \lambda_2 \\ 1 \end{bmatrix}$.

Finally, $\mathbf{u_0} = \begin{bmatrix} F_1 \\ F_0 \end{bmatrix} = \begin{bmatrix} 1 \\ 0 \end{bmatrix} = c_1 \mathbf{x_1} + c_2 \mathbf{x_2}$ tells us that $c_1 = -c_2 = \frac{1}{5}$.

Because $\begin{bmatrix} F_{k + 1} \\ F_{k} \end{bmatrix} = \mathbf{u_k} = c_1 \lambda_1^k x_1 + c_2 \lambda_2^k x_2$, we get:

$$
F_k = \frac{1}{\sqrt{5}}(\frac{1 + \sqrt{5}}{2})^k - \frac{1}{\sqrt{5}}(\frac{1 - \sqrt{5}}{2})^k
$$

Using eigenvalues and eigenvectors, we have found a `closed form expression` for the Fibonacci numbers.

## Summary

When a sequence evolves over time according to the rules of a `first order system`, the eigenvalues of the matrix of that system determine the long term behavior of the series. To get an exact formula for the series we find the eigenvectors of the matrix and then solve for the coefficient $c_1, c_2, \cdots$.
