# The Four Fundamental Subspaces

In this lecture we discuss the four fundamental spaces associated with a matrix and the relations between them.

## Four subspaces

Any $m$ by $n$ matrix $A$ determines four subspaces (possibly containing only zero vectors);

### Column space, C(A)

$C(A)$ consists of all combinations of the columns of $A$ and is a vector space in $\mathbb{R}^m$.

### Nullspace, N(A)

This consists of all solutions $\mathbf{x}$ of the equation $A\mathbf{x} = \mathbf{0}$ and lies in $\mathbf{R}^n$.

### Row space, R(A^T)

The combinations of the row vectors of $A$ form a subspace of $\mathbf{R}^n$. We equate this with $C(A^T)$, the column space of the transpose of $A$.

### Left nullspace, N(A^T)

We call the nullspace of $A^T$ the `left nullspace` of $A$. This is a subspace of $\mathbb{R}^m$.

## Basis and Dimension

### Column space

The $r$ pivot columns form a basis for $C(A)$:

$$
\text{dim } C(A) = r
$$

### Nullspace

The special solutions to $A\mathbf{x} = \mathbf{0}$ correspond to free variables and form a basis for $N(A)$. An $m$ by $n$ matrix has $n - r$ free variables:

$$
\text{dim } N(A) = n - r
$$

### Row space

We could perform row reduction on $A^T$, but instead we make use of $R$, the row reduced echelon form of $A$.

$$
A =
\begin{bmatrix}
1 & 2 & 3 & 1 \\
1 & 1 & 2 & 1 \\
1 & 2 & 3 & 1 \\
\end{bmatrix}
\to
\cdots
\to
\begin{bmatrix}
1 & 0 & 1 & 1 \\
0 & 1 & 1 & 0 \\
0 & 0 & 0 & 0 \\
\end{bmatrix}
=
\begin{bmatrix}
I & F \\
0 & 0 \\
\end{bmatrix}
= R
$$

Although the column spaces of $A$ and $R$ are different, the row space of $R$ is the same as the row space of $A$. The rows of $R$ are combinations of the rows of $A$, and because reduction is reversible the rows of $A$ are combinations of the rows of $R$.

The first $r$ rows of $R$ are the `echelon` basis for the row space of $A$:

$$
\text{dim } C(A^T) = r
$$

### Left nullspace

The matrix $A^T$ has $m$ columns. We just saw that $r$ is the rank of $A^T$, so the number of free columns of $A^T$ must be $m - r$:

$$
\text{dim } N(A^T) = m - r
$$

The left nullspace is the collection of vectors $y$ for which $A^Ty = 0$. Equivalently, $y^TA = 0$; there $y$ and $0$ are row vectors. We say `left nullspace` because $y^T$ is on the left of $A$ in this equation.

To find a basis for the left nullspace we reduce an augmented version of $A$:

$$
\begin{bmatrix}
A_{m \times n} & I_{m \times n}
\end{bmatrix}
\to
\begin{bmatrix}
R_{m \times n} & E_{m \times n}
\end{bmatrix}
$$

From this we get the matrix $E$ for which $EA = R$. (If $A$ is a square, invertible matrix then $E = A^{-1}$.) In our example,

$$
EA =
\begin{bmatrix}
-1 & 2 & 0 \\
1 & -1 & 0 \\
-1 & 0 & 1 \\
\end{bmatrix}
\begin{bmatrix}
1 & 2 & 3 & 1\\
1 & 1 & 2 & 1\\
1 & 2 & 3 & 1\\
\end{bmatrix}
=
\begin{bmatrix}
1 & 0 & 1 & 1\\
0 & 1 & 1 & 0\\
0 & 0 & 0 & 0\\
\end{bmatrix}
=R
$$

The bottom $m - r$ rows of $E$ describe linear dependencies of rows of $A$, because the bottom $m - r$ rows of $R$ are zero. Here $m - r = 1$(one zero row in R).

The bottom $m - r$ rows of $E$ satisfy the equation $\mathbf{y}^TA=\mathbf{0}$ and form a basis for the left nullspace of $A$.

## New vector space

The collection of all $3 \times 3$ matrices forms a vector space; call it $M$. We can add matrices and multiply them by scalars and there's a zero matrix(additive identity). If we ignore the fact that we can multiply matrices by each other, they behave just like vectors.

Some subspace of $M$ include:

- all upper trianglar matrices;
- all symmetric matrices;
- D, all diagonal matrices

$D$ is the intersection of the first two spaces. Its dimension is $3$; one basis for $D$ is:

$$
\begin{bmatrix}
1 & 0 & 0 \\
0 & 0 & 0 \\
0 & 0 & 0 \\
\end{bmatrix}
,
\begin{bmatrix}
0 & 0 & 0 \\
0 & 3 & 0 \\
0 & 0 & 0 \\
\end{bmatrix}
,
\begin{bmatrix}
0 & 0 & 0 \\
0 & 0 & 0 \\
0 & 0 & 7 \\
\end{bmatrix}
$$
