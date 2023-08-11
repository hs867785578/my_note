# Orthogonal Matrices and Gram-Schmidt

In this lecture we finish introducing orthogonality. Using an orthonormal basis or a matrix with orthonormal columns makes calculations much easier. The Gram-Schmidt process starts with any basis and produces an orthonormal basis that spans the same space as the original basis.

## Orthonormal Vectors
The vectors $\mathbf{q_1}, \mathbf{q_2}, \cdots, \mathbf{q_n}$ are `orthonormal` if:

$$
\mathbf{q_i}^T \mathbf{q_j} =
\begin{cases}
0,\text{ if } i \ne j \\
1,\text{ if } i = j \\
\end{cases}
$$

In other words, they all have (normal) length $1$ and are perpendicular(orthonormal) to each other. Orthonormal vectors are always independent.

## Orthonormal Matrix

If the columns of $Q = [\mathbf{q_1} \cdots \mathbf{q_n}]$ are orthonormal, then $Q^TQ = I$ is the identity.

Matrices with orthonormal columns are a new class of important matrices to add to those on our list:

- triangular
- diagonal
- permutation
- symmetric
- reduced row echelon
- projection

We'll call them `orthonormal matrices`.

A square orthonormal matrix $Q$ is called an `orthogonal matrix`. If $Q$ is square, then $Q^TQ = I$ tells us that:

$$
Q^T = Q^{-1}
$$

For example, if $Q = \begin{bmatrix} 0 & 0 & 1 \\ 1 & 0 & 0 \\ 0 & 1 & 0 \end{bmatrix}$ then $Q^T = \begin{bmatrix} 0 & 1 & 0 \\ 0 & 0 & 1 \\ 1 & 0 & 0 \end{bmatrix}$. Both $Q$ and $Q^T$ are orthogonal matrices, and their product is the identity.

The matrix $Q = \begin{bmatrix} cos\theta & -sin\theta \\ sin\theta & cos\theta \end{bmatrix}$ is orthogonal. The matrix $\begin{bmatrix} 1 & 1 \\ 1 & -1 \end{bmatrix}$ is not, but we can adjust that matrix to get the orthogonal matrix $Q = \frac{1}{\sqrt{2}} \begin{bmatrix} 1 & 1 \\ 1 & -1 \end{bmatrix}$. We can use the same tactic to find some larger orthogonal matrices called `Hadamard matrices`:

$$
Q = \frac{1}{2}
\begin{bmatrix}
1 & 1 & 1 & 1 \\
1 & -1 & 1 & -1 \\
1 & 1 & -1 & -1 \\
1 & -1 & -1 & 1 \\
\end{bmatrix}
$$

An example of a rectangular matrix with orthonormal columns is:

$$
Q = \frac{1}{3}
\begin{bmatrix}
1 & -2 \\
2 & -1 \\
2 & 2 \\
\end{bmatrix}
$$

We can extend this to a (square) orthogonal matrix:

$$
\frac{1}{3}
\begin{bmatrix}
1 & -2 & 2 \\
2 & -1 & -2 \\
2 & 2 & 1 \\
\end{bmatrix}
$$

These examples are particularly nice because they don't include complicated square roots.

## Orthonormal Columns are Good

Suppose $Q$ has orthonormal columns, The matrix that projects onto the column space of $Q$ is:

$$
P = Q(Q^TQ)^{-1}Q^T
$$

If the columns of $Q$ are orthonormal, then $Q^TQ = I$ and $P = QQ^T$. If $Q$ is square, then $P = I$ because the columns of $Q$ span the entire space.

Many equations become trivial when using a matrix with orthonormal columns. If our basis is orthonormal, the projection component $\hat{x_i}$ is just $\mathbf{q_i}^T\mathbf{b}$ because $A^TA\hat{x} = A^T\mathbf{b}$ becomes $\mathbf{\hat{x}} = Q^T\mathbf{b}$.

## Gram-Schmidt

With elimination, our goal was `make the matrix triangular`. Now our goal is `make the matrix orthonormal`.

We start with two independent vectors $\mathbf{a}$ and $\mathbf{b}$ and want to find orthonormal vectors $\mathbf{q_1}$ and $\mathbf{q_2}$ that span the same plane. We start by finding orthogonal vectors $\mathbf{A}$ and $\mathbf{B}$ that span the same space as $\mathbf{a}$ and $\mathbf{b}$. Then the unit vectors $\mathbf{q_1} = \frac{\mathbf{A}}{||\mathbf{A}||}$ and $\mathbf{q_2} = \frac{\mathbf{B}}{||\mathbf{B}||}$ from the desired orthonormal basis.

Let $\mathbf{A} = \mathbf{a}$. We get a vector orthogonal to $\mathbf{A}$ in the space spanned by $\mathbf{a}$ and $\mathbf{b}$ by projecting $\mathbf{b}$ onto $\mathbf{a}$ and letting $\mathbf{B} = \mathbf{b} - \mathbf{p}$. ($\mathbf{B}$ is what we previously called $\mathbf{e}$.)

$$
\mathbf{B} = \mathbf{b} - \frac{\mathbf{A}^T\mathbf{b}}{\mathbf{A}^T\mathbf{A}} \mathbf{A}
$$

If we multiply both sides of this equation by $\mathbf{A}^T$, we see that $\mathbf{A}^T\mathbf{B} = \mathbf{0}$

What if we had started with three independent vectors, $\mathbf{a}$, $\mathbf{b}$ and $\mathbf{c}$? Then we'd find a vector $\mathbf{C}$ orthogonal to both $\mathbf{A}$ and $\mathbf{B}$ by subtracting from $\mathbf{c}$ its components in the $\mathbf{A}$ and $\mathbf{B}$ directions:

$$
\mathbf{C} = \mathbf{c} - \frac{\mathbf{A}^T \mathbf{c}} {\mathbf{A}^T\mathbf{A}} \mathbf{A} - \frac{\mathbf{B}^T \mathbf{c}} {\mathbf{B}^T \mathbf{B}} \mathbf{B}
$$

For example, suppose $\mathbf{a} = \begin{bmatrix} 1 \\ 1 \\ 1 \end{bmatrix}$ and $\mathbf{b} = \begin{bmatrix} 1 \\ 0 \\ 2 \end{bmatrix}$. Then $\mathbf{A} = \mathbf{a}$ and:

$$
\begin{align}
\mathbf{B} & = \begin{bmatrix} 1 \\ 0 \\ 2 \end{bmatrix} - \frac{\mathbf{A}^T \mathbf{b}} {\mathbf{A}^T\mathbf{A}} \begin{bmatrix} 1 \\ 1 \\ 1 \end{bmatrix} \\
& = \begin{bmatrix} 1 \\ 0 \\ 2 \end{bmatrix} - \frac{3}{3} \begin{bmatrix} 1 \\ 1 \\ 1 \end{bmatrix} \\
& = \begin{bmatrix} 0 \\ -1 \\ 1 \end{bmatrix}
\end{align}
$$

Normalizing, we get:

$$
Q = [ \mathbf{q_1} \mathbf{q_2}] =
\begin{bmatrix}
1/\sqrt{3} & 0 \\
1/\sqrt{3} & -1/\sqrt{2} \\
1/\sqrt{3} & 1/\sqrt{2} \\
\end{bmatrix}
$$

The column space of $Q$ is the plane spanned by $\mathbf{a}$ and $\mathbf{b}$.

When we studied elimination, we wrote the process in terms of matrices and found $A = LU$. A similar equation $A = QR$ relates our starting matrix $A$ to the result $Q$ of the Gram-Schmidt process. Where $L$ was lower triangular, $R$ is upper triangular.

Suppose $A = [ \mathbf{a_1} \mathbf{a_2} ]$, then:

$$
A = Q R
$$

$$
\begin{bmatrix} \mathbf{a_1} & \mathbf{a_2} \end{bmatrix} = \begin{bmatrix} \mathbf{q_1} & \mathbf{q_2} \end{bmatrix}
\begin{bmatrix}
\mathbf{a_1}^T\mathbf{q_1} & \mathbf{a_2}^T\mathbf{q_1} \\
\mathbf{a_1}^T\mathbf{q_2} & \mathbf{a_2}^T\mathbf{q_2} \\
\end{bmatrix}
$$

If $R$ is upper triangular, then it should be true that $\mathbf{a_1}^T\mathbf{q_2} = 0$. This must be true because we chose $\mathbf{q_1}$ to be a unit vector in the direction of $\mathbf{a_1}$. All the latter $\mathbf{q_i}$ were chosen to be perpendicular to the earlier ones.

Notice that $R = Q^TA$. This makes sense; $Q^TQ = I$.
