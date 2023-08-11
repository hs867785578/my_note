# Transpose, permutations, spaces R^n

In this lecture we introduce vector spaces and their subspaces.

## Permutations
Multiplication by a permutation matrix $P$ swaps the rows of a matrix; when applying the method of elimination we use permutation matrices to move zeros out of pivot positions. Our factorization $A = LU$ then becomes $PA = LU$, where $P$ is a permutation matrix which reorders any number of rows of $A$. Recall that $P^{-1} = P^T$, i.e. that $P^TP = I$.

## Transposes
When we take the transpose of a matrix, its rows become columns and its columns become rows. If we denote the entry in row $i$ and column $j$ of matrix $A$ by $A_{ij}$, then we can describe $A^T$ by $A^T_{ij} = A_{ij}$. For example:

$$
\begin{bmatrix}
1 & 3 \\
2 & 3 \\
4 & 1 \\
\end{bmatrix}^T
=
\begin{bmatrix}
1 & 2 & 4\\
3 & 3 & 1\\
\end{bmatrix}
$$

A matrix $A$ is `symmetric` if $A^T = A$. Given any matrix $R$(not necessarily square) the product $R^TR$ is always symmetric, because $(R^TR)^T = R^T(R^T)^T = R^TR$.(Note that $(R^T)^T = R$.)

## Vector spaces
We can add vectors and multiply them by numbers, which means we can discuss `linear combinations` of vectors. These combinationss follow the rules of a `vector space`.

One such vector space is $\mathbb{R}^2$, the set of all vectors with exactly two real number components. We depict the vector $\begin{bmatrix}a \\ b \end{bmatrix}$ by drawing an arrow from the origin to the point $(a, b)$ which is $a$ units to the right of the origin and $b$ units above it, and we call $\mathbb{R}^2$ the `x-y plane`.

Another example of a space is $\mathbb{R}^n$, the set of (column) vectors with $n$ real number components.

## Closure
The collection of vectors with exactly two `positive` real valued components is `not` a vector space. The sum of any two vectors in that collection is again in the collection, but multiplying any vector by, say, $-5$, gives a vector that's not in the collection. We say that this collection of positive vectors is `closed` under addition but not under multiplication.

If a collection of vectors is closed under linear combinations(i.e. under addtion and multiplication by any real numbers), and if multiplication and addtion behave in a reasonable way, then we call that collection a `vector space`.

## Subspaces

A vector space that is contained inside of another vector space is called a `subspace` of that space. For example, take any non-zero vector $\mathbf{v}$ in $\mathbb{R}^2$. Then the set of all vectors $c\mathbf{v}$, where $c$ is a real number, forms a subspace of $\mathbb{R}^2$. This collection of vectors describes a line through $\begin{bmatrix}0 \\ 0\end{bmatrix}$ in $\mathbb{R}^2$ and is closed under addition.

A line in $\mathbb{R}^2$ that does not pass through the origin is `not` a subspace of $\mathbb{R}^2$. Multiplying any vector on that line by $0$ gives the zero vector, which does not lie on the line. Every subspace must contain the zero vector because vector spaces are closed under multiplication.

The subspaces of $\mathbb{R}^2$ are:

1. all of $\mathbb{R}^2$;
2. any line through $\begin{bmatrix} 0 \\ 0\end{bmatrix}$;
3. the zero vector($Z$) alone.

The subspaces of $\mathbb{R}^3$ are:

1. all of $\mathbb{R}^3$;
2. any plane through $\begin{bmatrix} 0 \\ 0 \\ 0\end{bmatrix}$;
2. any line through $\begin{bmatrix} 0 \\ 0 \\ 0\end{bmatrix}$;
3. the zero vector($Z$) alone.

## Column space

Given a matrix $A$ with columns in $\mathbb{R}^3$, these columns and all their linear combinations form a subspace of $\mathbb{R}^3$. This is the `column space` $C(A)$. If $A = \begin{bmatrix} 1 & 3 \\ 2 & 3 \\ 4 & 1 \end{bmatrix}$, the column space of $A$ is the plane through the origin in $\mathbb{R}^3$ containing $\begin{bmatrix} 1 \\ 2 \\ 4\end{bmatrix}$ and $\begin{bmatrix} 3 \\ 3 \\ 1\end{bmatrix}$.

Our next task will be to understand the equation $A\mathbf{x} = \mathbf{b}$ in terms of subspaces and the column space of $A$.
