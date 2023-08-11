# Projections onto Subspaces

## Projection

If we have a vector $\mathbf{b}$ and a line determined by a vector $\mathbf{a}$, how do we find the point on the line that is closet to $\mathbf{b}$?

![projection](images/projection/proj_2d.png)

We can see from the figure that this closet point $p$ is at the intersection formed by a line through $\mathbf{b}$ that is orthogonal to $\mathbf{a}$. If we think of $\mathbf{p}$ as an approximation of $\mathbf{b}$, then the length of $\mathbf{e} = \mathbf{b} - \mathbf{p}$ is the error in that approximation.

We could try to find $\mathbf{p}$ using trigonometry or calculus, but it's easier to use linear algebra. Since $\mathbf{p}$ lies on the line through $\mathbf{a}$, we know $\mathbf{p} = \mathbf{a}x$ for some number $x$. We also know that $\mathbf{a}$ is perpendicular to $\mathbf{e} = \mathbf{b} - \mathbf{a}x$:

$$
\begin{align}
\mathbf{a}^T (\mathbf{b} - \mathbf{a}x) &= 0 \\
\mathbf{a}^T\mathbf{a}x &= \mathbf{a}^T\mathbf{b} \\
x &= \frac{\mathbf{a}^T\mathbf{b}}{\mathbf{a}^T\mathbf{a}}
\end{align}
$$

and

$$
\mathbf{p} = \mathbf{a}x = \mathbf{a} \frac{\mathbf{a}^T\mathbf{b}}{\mathbf{a}^T\mathbf{a}}
$$

Doubling $\mathbf{b}$ will double $\mathbf{p}$, doubling $\mathbf{a}$ does not affect $\mathbf{p}$.

## Projection Matrix

We'd like to write this projection in terms of a `projection matrix` $P$:

$$
\mathbf{p} = P\mathbf{b}
$$

As

$$
\mathbf{p} = \mathbf{a}x = \mathbf{a} \frac{\mathbf{a}^T\mathbf{b}}{\mathbf{a}^T\mathbf{a}}
$$

so the matrix is:

$$
P = \frac{\mathbf{a}\mathbf{a}^T}{\mathbf{a}^T\mathbf{a}}
$$

Note that $\mathbf{a}\mathbf{a}^T$ is a $3 \times 3$ matrix, not a number; matrix multiplication is not commutative.

The column space of $P$ is spanned by $\mathbf{a}$ because for any $\mathbf{b}$, $P\mathbf{b}$ lies on the line determined by $\mathbf{a}$. The rank of $P$ is $1$. $P$ is symmetric. $P^2\mathbf{b} = P\mathbf{b}$ because the projection of a vector already on the line through $\mathbf{a}$ is just that vector. In general, projection matrices have the properties:

$$
P^T = P
$$

$$
P^2 = P
$$

## Why Projection?
As we know, the equation $A\mathbf{x} = \mathbf{b}$ may have no solution. The vector $A\mathbf{x}$ is always in the column space of $A$, and $\mathbf{b}$ is unlikely to be in the column space. So, we project $\mathbf{b}$ onto a vector $\mathbf{p}$ in the column space of $A$ and solve $A\mathbf{\hat{x}} = \mathbf{p}$.

## Projection in Higher Dimension
In $\mathbb{R}^3$, how do we project a vector $\mathbf{b}$ onto the closet point $\mathbf{p}$ in a plane?

If $\mathbf{a_1}$ and $\mathbf{a_2}$ form a basis for the plane, then that plane is the column space of the matrix $A = \begin{bmatrix} \mathbf{a_1} & \mathbf{a_2} \end{bmatrix}$.

We know that $\mathbf{b} = \hat{x_1}\mathbf{a_1} + \hat{x_2}\mathbf{a_2} = A\mathbf{\hat{x}}$. We want to find $\mathbf{\hat{x}}$. There are many ways to show that $\mathbf{e} = \mathbf{b} - \mathbf{p} = \mathbf{b} - A\mathbf{\hat{x}}$ is orthogonal to the plane we're projecting onto, after wihch we can use the fact that $\mathbf{e}$ is perpendicular to $\mathbf{a_1}$ and $\mathbf{a_2}$:

$$
\begin{align}
\mathbf{a_1}^T (\mathbf{b} - A\mathbf{\hat{x}}) = 0 \\
\mathbf{a_2}^T (\mathbf{b} - A\mathbf{\hat{x}}) = 0 \\
\end{align}
$$

In matrix form:

$$
A^T (\mathbf{b} - A \mathbf{\hat{x}}) = 0
$$

When we were projecting onto a line, $A$ only had one column and so this equation looked like: 

$$
A^T (\mathbf{b} - A \mathbf{\hat{x}}) = 0
$$

Note that $\mathbf{e} = \mathbf{b} - A\mathbf{\hat{x}}$ is in the nullspace of $A^T$ and so is in the left nullspace of $A$. We know that everything in the left nullspace of $A$ is perpendicular to the column space of $A$, so this is another confirmation that our calculation are correct.

We can rewrite the equation $A^T(\mathbf{b} - A\mathbf{\hat{x}})$ as:

$$
A^TA\mathbf{\hat{x}} = A^T \mathbf{b}
$$

When projecting onto a line, $A^TA$ was just a number; now it is a square matrix.

So instead of dividing by $\mathbf{a}^T\mathbf{a}$ we now have to multiply by $(A^TA)^{-1}$.

In $n$ dimensions:

$$
\begin{align}
\mathbf{\hat{x}} &= (A^TA)^{-1}A^T\mathbf{b} \\
\mathbf{p} = A\mathbf{\hat{x}} &= A(A^TA)^{-1}A^T\mathbf{b} \\
P &= A(A^TA)^{-1}A^T
\end{align}
$$

It's tempting to try to simplify these expressions, but if $A$ isn't a square matrix we can't say that $(A^TA)^{-1} = A^{-1}(A^T)^{-1}$.

If $A$ does happen to be a square, invertible matrix then it's column space is the whole space and contains $\mathbf{b}$. In this case $P$ is the identity, as we find when we simplify. It is still true that:

$$
\begin{align}
P^T = P \\
P^2 = P \\
\end{align}
$$

## Least Squares

![least square](images/projection/least_square.png)

Suppose we're given a collection of data points $(t, b)$:

$$
\{(1, 1), (2, 2), (3, 2)\}
$$

and we want to find the closet line $b = C + Dt$ to that collection. If the line went through all three points, we'd have:

$$
\begin{align}
C + D = 1 \\
C + 2D = 2 \\
C + 3D = 2 \\
\end{align}
$$

Which is equivalent to:

$$
\begin{bmatrix}
1 & 1 \\
1 & 2 \\
1 & 3 \\
\end{bmatrix}
\begin{bmatrix}
C \\
D \\
\end{bmatrix}
=
\begin{bmatrix}
1 \\
2 \\
2 \\
\end{bmatrix}
$$

$$
A \mathbf{x} = \mathbf{b}
$$

In our example the line does not go through all three points, so this equation is not solvable. Instead we'll solve:

$$
A^TA\mathbf{\hat{x}} = A^T \mathbf{b}
$$
