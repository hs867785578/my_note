# Column Space and Nullspace

In this lecture we continue to study subspaces, particularly the column space and nullspace of a matrix.

## Review of subspaces

A vector space is a collection of vectors which is closed under linear combinations. In other words, for any two vectors $\mathbf{v}$ and $\mathbf{w}$ in the space and any two real numbers $c$ and $d$, the vector $c\mathbf{v} + d\mathbf{w}$ is also in the vector space. A subspace is a vector space contianed inside a vector space.

A plane $P$ containing $\begin{bmatrix} 0 \\ 0 \\ 0 \end{bmatrix}$ and a line $L$ containing $\begin{bmatrix} 0 \\ 0 \\ 0 \end{bmatrix}$ are both subspaces of $\mathbb{R}^3$. The union $P \cup L$ of those two subspaces is generally not a subspace, because the sum of a vector in $P$ and a vector in $L$ is probably not contained in $P \cup L$. The intersection $S \cap T$ of two subspace $S$ and $T$ is a subspace. To prove this, use the fact that both $S$ and $T$ are closed under linear combinations to show that their intersection is closed under linear combinations.

## Column space of A

The `column space` of a matrix $A$ is the vector space made up of all linear combinations of the columns of $A$.

### Solving Ax = b

Given a matrix $A$, for what vectors $\mathbf{b}$ does $A\mathbf{x} = \mathbf{b}$ have a solution $\mathbf{x}$?

$$
A =
\begin{bmatrix}
1 & 1 & 2 \\
2 & 1 & 3 \\
3 & 1 & 4 \\
4 & 1 & 5
\end{bmatrix}
$$

Then $A\mathbf{x} = \mathbf{b}$ does not have a solution for every choice of $\mathbf{b}$ because solving $A\mathbf{x} = \mathbf{b}$ is equivalent to solving four linear equations in three unknowns. If there is a solution $\mathbf{x}$ to $A\mathbf{x} = \mathbf{b}$, then $\mathbf{b}$ must be a linear combination of the columns of $A$. Only three columns cannot fill the entire four dimensional vector space, some vectors $\mathbf{b}$ can not be expressed as linear combinations of columns of $A$.

Big question: what $\mathbf{b}$'s allow $A\mathbf{x} = \mathbf{b}$ to be solved?

A useful approach is to choose $\mathbf{x}$ and find the vector $\mathbf{b} = A\mathbf{x}$  corresponding to that solution. The components of $\mathbf{x}$ are just the coefficients in a linear combination of columns of $A$.

The system of linear equations $A\mathbf{x} = \mathbf{b}$ is `solvable` exactly when $\mathbf{b}$ is a vector in the `column space` of $A$.

For our example matrix $A$, what can we say about the column space of $A$? Are the columns of $A$ `independent`? In other words, does each column contribute something new to the subspace?

The third column of $A$ is the sum of the first two columns, so does not add anything to the subspace. The column space of our matrix $A$ is a two dimensional subspace of $\mathbb{R}^4$.

## Nullspace of A

The `nullspace` of a matrix is the collection of all solutions $\mathbf{x} = \begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix}$ to the equation $A\mathbf{x} = \mathbf{0}$.

The column space of the matrix in our example was a subspace of $\mathbb{R}^4$. The nullspace of $A$ is a subspace of $\mathbb{R}^3$. To see that it's a vector space, check that any sum or multiple of solutions to $A\mathbf{x} = \mathbf{0}$ is also a solution:

$$
A(\mathbf{x}_ 1 + \mathbf{x}_ 2) = A\mathbf{x}_ 1 + A\mathbf{x}_ 2 = \mathbf{0} + \mathbf{0} = \mathbf{0}
$$

$$
A(c\mathbf{x}) = cA\mathbf{x} = \mathbf{0}
$$

In the example:

$$
\begin{bmatrix}
1 & 1 & 2 \\
2 & 1 & 3 \\
3 & 1 & 4 \\
4 & 1 & 5
\end{bmatrix}
\begin{bmatrix}
x_1 \\
x_2 \\
x_3
\end{bmatrix}
=
\begin{bmatrix}
0 \\
0 \\
0 \\
0
\end{bmatrix}
$$


The nullspace $N(A)$ consists of all multiples of $\begin{bmatrix} 1 \\ 1 \\ -1 \end{bmatrix}$; column 1 plus column 2 minus column 3 equals the zero vector. This nullspace is a line in $\mathbb{R}^3$.

### Other values of b

The solutions to the equation:

$$
\begin{bmatrix}
1 & 1 & 2 \\
2 & 1 & 3 \\
3 & 1 & 4 \\
4 & 1 & 5
\end{bmatrix}
\begin{bmatrix}
x_1 \\
x_2 \\
x_3
\end{bmatrix}
=
\begin{bmatrix}
1 \\
2 \\
3 \\
4
\end{bmatrix}
$$

do not form a subspace. The zero vector is not a solution to this equation. The set of solutions forms a line in $\mathbb{R}^3$ that passes through the points $\begin{bmatrix} 1 \\ 0 \\ 0  \end{bmatrix}$ and $\begin{bmatrix} 0 \\ -1 \\ 1 \end{bmatrix}$ but not $\begin{bmatrix} 0 \\ 0 \\ 0 \end{bmatrix}$.
