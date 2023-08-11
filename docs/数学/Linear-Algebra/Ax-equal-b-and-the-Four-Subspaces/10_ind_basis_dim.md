# Independence, Basis and Dimension

What does it mean for vectors to be independent? How does the idea of independence help us describe subspaces like the nullspace?

## Linear independence

Suppose $A$ is an $m$ by $n$ matrix with $m < n$(so $A\mathbf{x} = \mathbf{b}$ has more unknowns than equations). $A$ has at least one free variable, so there are nonzero solutions to $A\mathbf{x} = \mathbf{0}$. A combination of the columns is zero, so the columns of this $A$ are `dependent`.

We say vectors $\mathbf{x}_ 1, \mathbf{x}_ 2, \cdots, \mathbf{x}_n$ are `linear independent` (or just `independent`) if $c_1\mathbf{x}_ 1 + c_2\mathbf{x}_2 + \cdots + c_n\mathbf{x}_n = \mathbf{0}$ only when $c_1, c_2, \cdots, c_n$ are all $0$. When those vectors are the columns of $A$, the only solution to $A\mathbf{x} = \mathbf{0}$ is $\mathbf{x} = \mathbf{0}$.

Two vectors are independent if they do not lie on the same line. Three vectors are independent if they do not lie on the same plane. Thinking of $A\mathbf{x}$ as a linear combination of the column vectors of $A$, we see that the column vectors of $A$ are independent exactly when the nullspace of $A$ contains only the zero vector.

If the columns of $A$ are independent then all columns are pivot columns, the rank of $A$ is $n$, and there are no free variables. If the columns of $A$ are dependent then the rank of $A$ is less than $n$ and there are free variables.

## Spanning a space
Vectors $\mathbf{v}_1, \mathbf{v}_2, \cdots, \mathbf{v}_k$ `span` a space when the space consists of all combinations of those vectors. For example, the columns vectors of $A$ span the column space of $A$.

If vectors $\mathbf{v}_1, \mathbf{v}_2, \cdots, \mathbf{v}_k$ span a space $S$, then $S$ is the smallest space containing those vectors.

## Basis and dimension

A `basis` for a vector space is a sequence of vectors $\mathbf{v}_1, \mathbf{v}_2, \cdots, \mathbf{v}_d$ with two properties:

- $\mathbf{v}_1, \mathbf{v}_2, \cdots, \mathbf{v}_d$ are independent;
- $\mathbf{v}_1, \mathbf{v}_2, \cdots, \mathbf{v}_ d$ span the vector space.

The basis of a space tells us everything we need to know about that space.

### Example: $\mathbb{R}^3$

One basis for $\mathbb{R}^3$ is ${ \begin{bmatrix} 1 \\ 0 \\ 0 \end{bmatrix}, \begin{bmatrix} 0 \\ 1 \\ 0 \end{bmatrix}, \begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix}}$. These are independent because:

$$
c_1 \begin{bmatrix} 1 \\ 0 \\ 0 \end{bmatrix} +
c_2 \begin{bmatrix} 0 \\ 1 \\ 0 \end{bmatrix} +
c_3 \begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix} =
\begin{bmatrix} 0 \\ 0 \\ 0 \end{bmatrix}
$$

is only possible when $c_1 = c_2 = c_3 = 0$. These vectors span $\mathbb{R}^3$.

As discussed at previous lecture, the vector $\begin{bmatrix} 1 \\ 1 \\ 2 \end{bmatrix}$, $\begin{bmatrix} 2 \\ 2 \\ 5 \end{bmatrix}$ and $\begin{bmatrix} 3 \\ 3 \\ 8 \end{bmatrix}$ do not form a basis for $\mathbb{R}^3$ because these are the column vectors of a matrix that has two identical rows. The three vectors are not linearly independent.

In general, $n$ vectors in $\mathbb{R}^n$ form a basis if they are the column vectors of an invertible matrix.

### Basis for a subspace

The vectors $\begin{bmatrix} 1 \\ 1 \\ 2 \end{bmatrix}$ and $\begin{bmatrix} 2 \\ 2 \\ 5 \end{bmatrix}$ span a plane in $\mathbb{R}^3$ but they cannot form a basis for $\mathbb{R}^3$. Given a space, every basis for that space has the same number of vectors; that number is the `dimension` of the space. So there are exactly $n$ vectors in every basis for $\mathbb{R}^n$.

## Bases of a column space and nullspace

Suppose:

$$
A = 
\begin{bmatrix}
1 & 2 & 3 & 1 \\
1 & 1 & 2 & 1 \\
1 & 2 & 3 & 1 \\
\end{bmatrix}
$$

By definition, the four column vectors of $A$ span the column space of $A$. The third and fourth column vectors are dependent on the first and second, and the first two columns are independent. Therefore, the first two column vectors are the pivot columns. They form a basis for the column space $C(A)$. The matrix has rank 2. In fact, for any matrix $A$ we can say:

$$
rank(A) = \text{number of pivot columns of } A = \text{dimension of } C(A)
$$

Note that matrices have a rank but not a dimension. Subspaces have a dimension but not a rank.

The column vectors of this $A$ are not independent, so the nullspace $N(A)$ contains more than just the zero vector. Because the third column is the sum of the first two, we know that the vector $\begin{bmatrix} -1 \\ -1 \\ 1 \\ 0 \end{bmatrix}$ is in the nullspace. Similarly, $\begin{bmatrix} -1 \\ 0 \\ 0 \\ 1 \end{bmatrix}$ is also in $N(A)$. These are the two special solutions to $A \mathbf{x} = \mathbf{0}$. We'll see that:

$$
\text{dimension of } N(A) = \text{number of free variables} = n - r
$$

so we know that the dimension of $N(A)$ is $4 - 2 = 2$. These two special solutions form a basisi for the nullspace.
