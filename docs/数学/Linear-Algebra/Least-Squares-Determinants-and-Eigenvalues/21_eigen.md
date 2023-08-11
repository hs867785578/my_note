# Eigenvalues and Eigenvectors

The subject of eigenvalues and eigenvectors will take up most of the rest of the course. We will again be working with square matrices. Eigenvalues are special numbers associated with a matrix and eigenvectors are special vectors.

## Eigenvectors and Eigenvalues

A matrix A `acts on` vectors $\mathbf{x}$ like a function does, with input $\mathbf{x}$ and output $A\mathbf{x}$. `Eigenvectors` are vectors for which $A\mathbf{x}$ is parallel to $\mathbf{x}$. In other words:

$$
A\mathbf{x} = \lambda \mathbf{x}
$$

In this equation, $\mathbf{x}$ is an `eigenvector` of A and $\lambda$ is an `eigenvalue` of A.

### Eigenvalue 0

If the eigenvalue $\lambda$ equals 0 then $A\mathbf{x} = 0 \mathbf{x} = \mathbf{0}$. Vectors with eigenvalue 0 make up the nullspace of A; if A is singular, then $\lambda = 0$ is an eigenvalue of A.

## Examples

Suppose P is the matrix of a projection onto a plane. For any $\mathbf{x}$ in the plane $P\mathbf{x} = \mathbf{0}$, so this is an eigenvector with eigenvalue $\lambda = 0$. The eigenvectors of P span the whole space(but his is not true for every matrix).

The matrix $B = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}$ has an eigenvector $\mathbf{x} = \begin{bmatrix} 1 \\ 1 \end{bmatrix}$ with eigenvalue 1 and another eigenvector $\mathbf{x} = \begin{bmatrix} 1 \\ -1 \end{bmatrix}$ with eigenvalue -1. These eigenvectors span the space. They are perpendicular because $B = B^T$ (as we will prove).

### det(A - \lambda I) = 0

An n by n matrix will have n eigenvalues, and their sum will be the sum of the diagonal entries of the matrix, if we know one eigenvalue we can use this fact to find the second.

Can we solve $A\mathbf{x} = \lambda \mathbf{x}$ for the eigenvalues and eigenvectors of A? Both $\lambda$ and $\mathbf{x}$ are unknown; we need to be clever to solve this problem:

$$
\begin{align}
A\mathbf{x} &= \lambda \mathbf{x} \\
(A - \lambda I) \mathbf{x} &= \mathbf{0}
\end{align}
$$

In order for $\mathbf{x}$ to be an eigenvector, $A - \lambda I$ must be singular. In other words, $det (A - \lambda I) = 0$. We can solve this `characteristic equation` for $\lambda$ to get n solutions.

If we're lucky, the solutions are distinct. If not, we have one or more `repeated eigenvalues`.

Once we've found an eigenvlaue $\lambda$, we can use elimination to find the nullspace of $A - \lambda I$. The vectors in that nullspace are eigenvectors of A with eigenvalue $\lambda$.

### Calculating Eigenvalues and Eigenvectors

Let $A = \begin{bmatrix} 3 & 1 \\ 1 & 3 \end{bmatrix}$, then:

$$
\begin{align}
det(A - \lambda I) & = \begin{vmatrix} 3 - \lambda & 1 \\ 1 & 3 - \lambda \end{vmatrix} \\
&= (3 - \lambda)^2 - 1 \\ 
&= \lambda^2 - 6\lambda + 8
\end{align}
$$

Note that the coefficient 6 is the trace (sum of diagonal entries) and 8 is the determinant of A. In general, the eigenvlaues of a 2 by 2 matrix are the solutions to:

$$
\lambda^2 - trac(A) * \lambda + det A = 0
$$

Just as the trace is the sum of the eigenvalues of a matrix, the product of the eigenvalues of any matrix equals its determinant.

For $A = \begin{bmatrix} 3 & 1 \\ 1 & 3 \end{bmatrix}$, the eigenvalues are $\lambda_1 = 4$ and $\lambda_2 = 2$. We find the eigenvectors $\mathbf{x_1} = \begin{bmatrix} 1 \\ 1 \end{bmatrix}$ for $\lambda_1 = 4$ in the nullspace of $A - \lambda_1 I = \begin{bmatrix} -1 & 1 \\ 1 & -1 \end{bmatrix}$.

$\mathbf{x_2}$ will be in the nullsapce of $A - 2I = \begin{bmatrix} 1 & 1 \\ 1 & 1 \end{bmatrix}$. The nullspace is an entire line; $\mathbf{x_2}$ could be any vector on that line. A natural choice is $\mathbf{x_2} = \begin{bmatrix} -1 \\ 1 \end{bmatrix}$.

Note that these eigenvectors are the same as those of $B = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}$. Adding $3I$ to the matrix $B = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}$ added 3 to each of its eigenvalues and did not change its eigenvectors, because $A\mathbf{x} = (B + 3I)\mathbf{x} = \lambda\mathbf{x} + 3\mathbf{x} = (\lambda + 3)\mathbf{x}$.

### A caution

Similarly, if $A\mathbf{x} = \lambda\mathbf{x}$ and $B\mathbf{x} = \alpha \mathbf{x}$, $(A + B)\mathbf{x} = (\lambda + \alpha)\mathbf{x}$. It would be nice if the eigenvalues of a matrix sum were always the sums of eigenvalues, but this is only true if A and B have the same eigenvalues. The eigenvalues of the product AB aren't usually equal to the products $\lambda(A)\lambda(B)$, either.

### Complex eigenvalues

The matrix $Q = \begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}$ rotates every vector in the plane by $90^{\circ}$. It has trace $0 = \lambda_{1} + \lambda_{2}$ and determinant $1 = \lambda_{1} * \lambda_{2}$. Its only real eigenvector is the zero vector; any other vector's direction changes when it is multiplied by Q. How will this affect our eigenvalue calculation?

$$
\begin{align}
det(A - \lambda I) &= \begin{vmatrix} - \lambda & - 1 \\ 1 & - \lambda \end{vmatrix} \\
&= \lambda^2 + 1
\end{align}
$$

$det (A - \lambda I) = 0$ has solutions $\lambda_1 = i$ and $\lambda_2 = -i$. If a matrix has a complex eigenvalues $a + bi$ then the `complex conjugate` $a - bi$ is also an eigenvalue of that matrix.

Symmetric matrices have real eigenvalues. For `antisymmetric` matrices like Q, for which $A^T = -A$, all eigenvalues are imaginary $(\lambda = bi)$.

### Triangular matrices and repeated eigenvalues

For triangular matrices such as $A = \begin{bmatrix} 3 & 1 \\ 0 & 3 \end{bmatrix}$, the eigenvalues are exactly the entries on the diagonal. In this case, the eigenvalues are 3 and 3:

$$
\begin{align}
det(A - \lambda det I) &= \begin{vmatrix} 3 - \lambda & 1 \\ 0 & 3 - \lambda \end{vmatrix} \\
&= (3 - \lambda)(3 - \lambda) \\
&= 0
\end{align}
$$

so $\lambda_{1} = 3$ and $\lambda_{2} = 3$. To find the eigenvectors, solve:

$$
(A - \lambda I) \mathbf{x} = \begin{bmatrix} 0 & 1 \\ 0 & 0 \end{bmatrix} \mathbf{x} = \mathbf{0}
$$

to get $\mathbf{x_1} = \begin{bmatrix} 1 \\ 0 \end{bmatrix}$. There is no independent eigenvector $\mathbf{x_2}$.
