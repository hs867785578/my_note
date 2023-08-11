# An Overview of Linear Algebra

Linear algebra progresses from vectors to matrices to subspaces.

## Vectors

What do you do with vectors? Take combinations.

We can multiply vectors by scalars, add, and subtract. Given vectors $\mathbf{u}$, $\mathbf{v}$ and $\mathbf{w}$ we can form the `linear combination` $x_1 \mathbf{u} + x_2\mathbf{v} + x_3\mathbf{w} = \mathbf{b}$.

An example in $\mathbb{R}^3$ would be:

$$
\mathbf{u} = \begin{bmatrix} 1 \\ -1 \\ 0 \end{bmatrix}, \mathbf{v} = \begin{bmatrix} 0 \\ 1 \\ -1 \end{bmatrix}, \mathbf{w} = \begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix}
$$

The collection of all multiples of $\mathbf{u}$ forms a line through the origin. The collection of all multiples of $\mathbf{v}$ forms another line. The collection of all combinations of $\mathbf{u}$ and $\mathbf{v}$ forms a plane. Taking all combinations of some vectors creates a `subspace`.

We could continue like this, or we can use a matrix to add in all multiples of $\mathbf{w}$.

## Matrices

Create a matrix $A$ with vectors $\mathbf{u}$, $\mathbf{v}$ and $\mathbf{w}$ in its colums:

$$
A =
\begin{bmatrix}
1 & 0 & 0 \\
-11 & 1 & 0 \\
0 & -1 & 1
\end{bmatrix}
$$

The product:

$$
A \mathbf{x} = 
\begin{bmatrix}
1 & 0 & 0 \\
-1 & 1 & 0 \\
0 & -1 & 1
\end{bmatrix}
\begin{bmatrix}
x_1 \\
x_2 \\
x_3
\end{bmatrix}
= 
\begin{bmatrix}
x_1 \\
-x_1 + x_2 \\
-x_2 + x_3
\end{bmatrix}
$$

equals the sum $x_1\mathbf{u} + x_2\mathbf{v} + x_3\mathbf{w} = \mathbf{b}$. The product of a matrix and a vector is a combination of the columns of the matrix. (This particular matrix A is a `difference matrix` because the components of $A\mathbf{x}$ are differences of the components of that vector.)

When we say $x_1 \mathbf{u} + x_2 \mathbf{v} + x_3 \mathbf{w} = \mathbf{b}$, we're thinking about multiplying numbers by vectors; when we say $A\mathbf{x} = \mathbf{b}$, we're thinking about multiplying a matrix(whose columns are $\mathbf{u}$, $\mathbf{v}$, and $\mathbf{w}$) by the numbers. The calculations are the same, but our perspective has changed.

For any input vector $\mathbf{x}$, the output of the operation "multiplication by A" is some vector $\mathbf{b}$:

$$
A \begin{bmatrix} 1 \\ 4 \\ 9 \end{bmatrix} = \begin{bmatrix} 1 \\ 3 \\ 5 \end{bmatrix}
$$

A deeper question is to start with a vector $\mathbf{b}$ and ask "for what vectors $\mathbf{x}$ does $A\mathbf{x} = \mathbf{b}$?" In our example, this means solving three equations in three unknowns, solving:

$$
A\mathbf{x} = \begin{bmatrix} 1 & 0 & \\ -1 & 1 & 0 \\ 0 & -1 & 1 \end{bmatrix} \begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix} = \begin{bmatrix} x_1 \\ x_2 - x_1 \\ x_3 - x_2 \end{bmatrix} = \begin{bmatrix} b_1 \\ b_2 \\ b_3 \end{bmatrix}
$$

is equivalent to solving:

$$
\begin{align}
x_1 = b_1 \\
x_2 - x_1 = b_2 \\
x_3 - x_2 = b_3
\end{align}
$$

In vector form, the solution is:

$$
\begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix} = \begin{bmatrix} b_1 \\ b_1 + b_2 \\ b_1 + b_2 + b_3 \end{bmatrix}
$$

But this just says:

$$
\mathbf{x} = \begin{bmatrix} 1 & 0 & 0 \\ 1 & 1 & 0 \\ 1 & 1 & 1 \end{bmatrix} \begin{bmatrix} b_1 \\ b_2 \\ b_3 \end{bmatrix}
$$

or

$$
\mathbf{x} = A^{-1} \mathbf{b}
$$

If the matrix $A$ is invertible, we can multiply on both sides by $A^{-1}$ to find the unique solution $\mathbf{x}$ to $A\mathbf{x} = \mathbf{b}$. We might say that $A$ represents a transform $\mathbf{x} \to \mathbf{b}$ that has an inverse transform $\mathbf{b} \to \mathbf{x}$.

In particular, if $\mathbf{b} = \begin{bmatrix} 0 \\ 0 \\ 0 \end{bmatrix}$ then $\mathbf{x} = \begin{bmatrix} 0 \\ 0 \\ 0 \end{bmatrix}$

The second example has the same columns $\mathbf{u}$ and $\mathbf{v}$ and replaces column vector $\mathbf{w}$:

$$
C = \begin{bmatrix} 1 & 0 & -1 \\ -1 & 1 & 0 \\ 0 & -1 & 1 \end{bmatrix}
$$

Then

$$
C\mathbf{x} = \begin{bmatrix} 1 & 0 & -1 \\ -1 & 1 & 0 \\ 0 & -1 & 1 \end{bmatrix} \begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix} = \begin{bmatrix} x_1 - x_3 \\ x_2 - x_1 \\ x_3 - x_2 \end{bmatrix}
$$

And our system of three equations in three unknowns become circular.

Where before $A\mathbf{x} = \mathbf{0}$ implied $\mathbf{x} = \mathbf{0}$, there are non-zero vectors $\mathbf{x}$ for which $C\mathbf{x} = \mathbf{0}$. For any vector $\mathbf{x}$ which $x_1 = x_2 = x_3, C\mathbf{x} = \mathbf{0}$. This is a significant difference; we can't multiply both sides of $C\mathbf{x} = \mathbf{0}$ by an inverse to find a non-zero solution $\mathbf{x}$.

The system of equations encoded in $C\mathbf{x} = \mathbf{b}$ is:

$$
\begin{align}
x_1 - x_3 = b_1 \\
x_2 - x_1 = b_2 \\
x_3 - x_2 = b_3
\end{align}
$$

If we all these three equations together, we get:

$$
0 = b_1 + b_2 + b_3
$$

This tells us that $C\mathbf{x} = \mathbf{b}$ has a solution $\mathbf{x}$ only when the components of $\mathbf{b}$ sum to $0$.

## Subspaces

Geometrically, the columns of $C$ lie in the same  plane(they are `dependent`, the columns of $A$ are `independent`). There are many vectors in $\mathbb{R}^3$ which do not lie in that plane. Those vectors cannot be written as a linear combination of the columns of $C$ and so correspond to values of $\mathbf{b}$ for which $C\mathbf{x} = \mathbf{b}$ has no solution $\mathbf{x}$. The linear combinations of the columns of $C$ form a two dimensional `subspace` of $\mathbb{R}^3$.

This plane of combinations of $\mathbf{u}$, $\mathbf{v}$ and $\mathbf{w}$ can be described as "all vectors $C\mathbf{x}$". But we know that the vectors $\mathbf{b}$ for which $C\mathbf{x} = \mathbf{b}$ satisfy the condition $b_1 + b_2 + b_3 = 0$. So the plane of all combinations of $\mathbf{u}$ and $\mathbf{v}$ consists of all vectors whose components sum to $0$.

If we take all combinations of:

$$
\mathbf{u} = \begin{bmatrix} 1 \\ -1 \\ 0\end{bmatrix}, \mathbf{v} = \begin{bmatrix} 0 \\ 1 \\ -1\end{bmatrix}, \mathbf{w} = \begin{bmatrix} 0 \\ 0 \\ 1\end{bmatrix}
$$

we get the entire space $\mathbb{R}^3$; the equation $A\mathbf{x} = \mathbf{b}$ has a solution for every $\mathbf{b}$ in $\mathbb{R}^3$. We say that $\mathbf{u}$, $\mathbf{v}$ and $\mathbf{w}$ form a `basis` for $\mathbb{R}^3$.

A `basis` for $\mathbb{R}^n$ is a collection of $n$ independent vectors in $\mathbb{R}^n$. Equaivalently, a `basis` is a collection of $n$ vectors whose combinations cover the whole space. Or, a collection of vectors forms a basis whenever a matrix which has those vectors as its columns is invertible.

A `vector space` is a collection of vectors that is closed under linear combinations. A `subspace` is a vector space inside another vector space; a plane through the origin in $\mathbb{R}^3$ is an example of a subspace. A subspace could be equal to the space it's contained in; the smallest subspace contians only the zero vector.

The subspace of $\mathbb{R}^3$ are:

- the origin;
- a line through the origin;
- a plane through the origin;
- all of $\mathbb{R}^3$.

## Conclusion
When you look at a matrix, try to see "what is it doing".

Matrices can be rectangular; we can have seven equations in three unknowns. Rectangular matrices are not invertible, but the symmetric, square matrix $A^TA$ that often appears when studying rectangular matrices may be invertible.
