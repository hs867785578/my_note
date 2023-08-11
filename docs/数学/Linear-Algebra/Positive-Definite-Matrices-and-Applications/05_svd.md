# Singular Value Decomposition(SVD)

The `Singular Value Decomposition` of a matrix is usually referred to as the SVD. This is the final and best factorization of a matrix:

$$
A = U \Sigma V^T
$$

where $U$ is the orthogonal, $\Sigma$ is diagonal, and $V$ is orthogonal.

In the decomposition $A = U \Sigma V^T$, A can be `any` matrix. We know that if A is symmetric positive definite its eigenvectors are orthogonal and we can write $A = Q \Lambda Q^T$. This is a spacial case of a SVD, with $U = V = Q$. For more general A, the SVD requires two different matrices U and V.

We've also learned how to write $A = S \Lambda S^{-1}$, where S is the matrix of n distinct eigenvectors of A. However, S may not be orthogonal; the matrices U and V in the SVD will be.

## How It Works

We can think of A as a linear transformation taking a vector $\mathbf{v}_ 1$ in its row space to a vector $\mathbf{u}_ 1 = A \mathbf{v}_ 1$ in its column space. The SVD arises from finding an orthogonal basis for the row space that gets transformed into orthogonal basis for the column space: $A\mathbf{v}_ i = \sigma_ i \mathbf{u}_ i$

It's not hard to find an orthogonal basis for the row space - the Gram-Schmidt process gives us one right way. But in general, there's no reason to expect A to transform that basis to another orthogonal basis.

You may be wondering about the vectors in the nullspace of A and $A^T$. There are no problem - zeros on the diagonal of $\Sigma$ will take care of them.

## Matrix Language

The heart of the problem is to find an orthogonal basis $\mathbf{v}_ 1, \mathbf{v}_ 2, \cdots, \mathbf{v}_ r$ for the row space of A for which:

$$
\begin{align}
A\begin{bmatrix}\mathbf{v}_ 1 & \mathbf{v}_ 2 & \cdots & \mathbf{v}_ r\end{bmatrix} &= 
\begin{bmatrix}\sigma_ 1 \mathbf{u}_ 1 & \sigma_2 \mathbf{u}_ 2 & \cdots & \sigma_r \mathbf{u}_ r\end{bmatrix} \\
&= \begin{bmatrix}\mathbf{u}_ 1 & \mathbf{u}_ 2 & \cdots & \mathbf{u}_ r\end{bmatrix} \begin{bmatrix}\sigma_1 & & & \\ & \sigma_2 & & \\ & & \ddots & \\ & & &\sigma_r \end{bmatrix}
\end{align}
$$

with $\mathbf{u}_ 1, \mathbf{u}_ 2, \cdots, \mathbf{u}_ r$ an orthonormal basis for the column space of A. Once we add in the nullspaces, this equation will become $AV = U\Sigma$. We can complete the orthonormal bases $\mathbf{v}_ 1, \mathbf{v}_ 2, \cdots, \mathbf{v}_ r$ and $\mathbf{u}_ 1, \mathbf{u}_ 2, \cdots, \mathbf{u}_ r$ to orthonormal bases for the entire space any way we want. Since $\mathbf{v}_ {r + 1}, \cdots, \mathbf{v}_ n$ will be in the nullspace of A, the diagonal entries $\sigma_{r + 1}, \cdots, \sigma_{n}$ will be 0.

The columns of $U$ and $V$ are bases for the row and column spaces, respectively. Usually $U \ne V$, but if A is positive definite we can use the same basis for its row and column space!

## Calculation

Suppose A is the invertible matrix $\begin{bmatrix}4 & 4 \\ -3 & 3\end{bmatrix}$. We want to find vectors $\mathbf{v}_ 1$ and $\mathbf{v}_ 2$ in the row space $\mathbb{R}^2$, $\mathbf{u}_ 1$ and $\mathbf{u}_ 2$ in the column space $\mathbb{R}^2$, and positive numbers $\sigma_1$ and $\sigma_2$ so that the $\mathbf{v}_ i$ are orthonormal, the $\mathbf{u}_ i$ are orthonormal, and the $\sigma_i$ are the scaling factors for which $A \mathbf{v}_ i = \sigma_i u_i$.

This is a big step toward finding orthonormal matrices $V$ and $U$ and a diagonal matrix $\Lambda$ for which:

$$
AV = U \Sigma
$$

Since V is orthogonal, we can multiply both sides by $V^{-1} = V^T$ to get:

$$
A = U \Sigma V^T
$$

Rather than solving for U, V and $\Sigma$ simultaneously, we multiply both sides by $A^T = V \Sigma ^T U^T$ to get:

$$
\begin{align}
A^T A &= V \Sigma U^{-1} U \Sigma V^T \\
&= V \Sigma^2 V^T \\
&= V \begin{bmatrix}\sigma^2_1 & & & \\ &\sigma^2_2 & & \\  & &\ddots & \\ & & & \sigma_n^2\end{bmatrix} V^T
\end{align}
$$

This is in the form $Q\Lambda Q^T$; we can now find V by diagonalizing the symmetric positive definite (or semidefinite) matrix $A^TA$. The columns of V are eigenvectors of $A^T$ and the eigenvalues of $A^TA$ are the values $\sigma_i^2$. We choose $\sigma_i$ to be the positive square root of $\lambda_i$.

To find U, we do the same thing with $AA^T$.

## SVD Example

We return to our matrix $A = \begin{bmatrix}4 & 4 \\ -3 & 3\end{bmatrix}$. We start by computing:

$$
\begin{align}
A^TA &=\begin{bmatrix}4 & -3 \\ 4 & 3\end{bmatrix}\begin{bmatrix}4 & 4\\ -3 & 3\end{bmatrix} \\
&= \begin{bmatrix}25 & 7 \\ 7 & 25\end{bmatrix}
\end{align}
$$

The eigenvectors of this matrix will give us the vector $\mathbf{v}_ i$, and the eigenvalues will gives us the numbers $\sigma_i$.

Two orthogonal eigenvectors of $A^TA$ are $\begin{bmatrix}1 \\ 1\end{bmatrix}$ and $\begin{bmatrix}1 \\ -1\end{bmatrix}$. To get an orthonormal basis, let $\mathbf{v}_ 1 = \begin{bmatrix}1 / \sqrt{2} \\ 1 / \sqrt{2}\end{bmatrix}$ and $\mathbf{v}_ 2 = \begin{bmatrix}1 / \sqrt{2} \\ -1 / \sqrt{2}\end{bmatrix}$. These have eigenvalues $\sigma_1^2 = 32$ and $\sigma_2^2 = 18$. We now have:

$$
A = U \Sigma V^T
$$

$$
\begin{bmatrix}4 & 4 \\ -3 & 3\end{bmatrix} = [] \begin{bmatrix}4 \sqrt{2} & 0 \\ 0 & 3\sqrt{2}\end{bmatrix} \begin{bmatrix}1 / \sqrt{2} & 1 / \sqrt{2} \\ 1 / \sqrt{2} & -1 / \sqrt{2}\end{bmatrix}
$$

We could solve this for U, but for practice we'll find U by finding orthonormal eigenvectors $\mathbf{u}_ 1$ and $\mathbf{u}_ 2$ for $AA^T = U \Sigma^2 U^T$.

$$
\begin{align}
AA^T &= \begin{bmatrix}4 & 4 \\ -3 & 3\end{bmatrix} \begin{bmatrix}4 & -3 \\ 4 & 3\end{bmatrix} \\
     &= \begin{bmatrix}32 & 0 \\ 0 & 18\end{bmatrix}
\end{align}
$$

Luckily, $AA^T$ happens to be diagonal. It's tempting to let $\mathbf{u}_ 1 = \begin{bmatrix}1 \\ 0\end{bmatrix}$ and $\mathbf{u}_ 2 = \begin{bmatrix}0 \\ 1\end{bmatrix}$, as Professor Strang did in the lecture, but because $A\mathbf{v}_ 2 = \begin{bmatrix}0 \\ -3 \sqrt{2}\end{bmatrix}$ we instead have $\mathbf{u}_ 2 = \begin{bmatrix}0 \\ -1\end{bmatrix}$ and $U = \begin{bmatrix}1 & 0 \\ 0 & -1\end{bmatrix}$. Note that this also gives us a chance to double check our calculation of $\sigma_1$ and $\sigma_2$.

Thus, the SVD of A is:

$$
A = U \Sigma V^T
$$

$$
\begin{bmatrix}4 & 4 \\ -3 & 3\end{bmatrix} = \begin{bmatrix}1 & 0 \\ 0 & -1\end{bmatrix} \begin{bmatrix}4\sqrt{2} & 0 \\ 0 & 3 \sqrt{2}\end{bmatrix} \begin{bmatrix}1/\sqrt{2} & 1/\sqrt{2} \\ 1 / \sqrt{2} & -1 / \sqrt{2}\end{bmatrix}.
$$

## Example with a Nullspace

Now let $A = \begin{bmatrix}4 & 3 \\ 8 & 6\end{bmatrix}$. This has a one dimensional nullspace and one dimensional row and column spaces.

The row space of A consists of the multiples of $\begin{bmatrix}4 \\ 3\end{bmatrix}$. The column space of A is made up of multiples of $\begin{bmatrix}4 \\ 8\end{bmatrix}$. The nullspace and left nullspace are perpendicular to the row and column spaces, respectively.

Unit basis vectors of the row and column spaces are are $\mathbf{v}_ 1 = \begin{bmatrix}0.8 \\ 0.6\end{bmatrix}$ and $\mathbf{u}_ 1 = \begin{bmatrix}1/\sqrt{5} \\ 1 / \sqrt{5}\end{bmatrix}$. To compute $\sigma_1$ we find the nonzero eigenvalue of $A^TA$.

$$
\begin{align}
A^TA &= \begin{bmatrix}4 & 8 \\ 3 & 6\end{bmatrix} \begin{bmatrix}4 & 3 \\ 8 & 6\end{bmatrix} \\
     &= \begin{bmatrix}80 & 60 \\ 60 & 45\end{bmatrix}
\end{align}
$$

Because this is a rank 1 matrix, one eigenvalue must be 0. The other must equal the trace, so $\sigma_1^2 = 125$. After finding unit vectors perpendicular to $\mathbf{u}_ 1$ and $\mathbf{v}_ 1$ (basis vectors for the left nullspace and nullspace, respectively) we see that the SVD of A is:

$$
\begin{bmatrix}4 & 3 \\ 8 & 6\end{bmatrix} = \frac{1}{\sqrt{5}}\begin{bmatrix}1 & 2 \\ 2 & -1\end{bmatrix} \begin{bmatrix}\sqrt{125} & 0 \\ 0 & 0\end{bmatrix}\begin{bmatrix}0.8 & 0.6 \\ 0.6 & -0.8\end{bmatrix}
$$

$$
A = U \Sigma V^T
$$

The singular value decomposition combines topics in linear algebra ranging from positive definite matrices to the four fundamental subspaces.

- $\mathbf{v}_ 1, \mathbf{v}_ 2, \cdots, \mathbf{v}_ r$ is an orthonormal basis for the row space;
- $\mathbf{u}_ 1, \mathbf{u}_ 2, \cdots, \mathbf{u}_ r$ is an orthonormal basis for the column space;
- $\mathbf{v}_ {r + 1}, \mathbf{v}_ {r + 2}, \cdots, \mathbf{v}_ n$ is an orthonormal basis for the nullspace;
- $\mathbf{u}_ {r + 1}, \mathbf{u}_ {r + 2}, \cdots, \mathbf{u}_ m$ is an orthonormal basis for the left nullspace.

These are the "right" bases to use, bacause $A\mathbf{v}_ i = \sigma_i \mathbf{u}_ i$.
