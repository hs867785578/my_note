# Solving Ax = b: row reduced form R

When does $A\mathbf{x} = \mathbf{b}$ have solutions $\mathbf{x}$, and how can we describe those solutions?

## Solvability conditions on b

We again use the exmaple:

$$
A =
\begin{bmatrix}
1 & 2 & 2 & 2 \\
2 & 4 & 6 & 8 \\
3 & 6 & 8 & 10
\end{bmatrix}
$$

The third row of $A$ is the sum of its first and second rows, so we know that if $A\mathbf{x} = \mathbf{b}$ the third component of $\mathbf{b}$ equals the sum of its first and second components. If $\mathbf{b}$ does not satisfy $b_3 = b_1 + b_2$ the system has no solution. If a combination of the rows of $A$ gives the zero row, then the same combination of the entries of $\mathbf{b}$ must equal zero.

One way to find out whether $A\mathbf{x} = \mathbf{b}$ is solvable is to use elimination on the augmented matrix. If a row of $A$ is completely eliminated, so is the corresponding entry in $\mathbf{b}$. In our example, row 3 of $A$ is completely eliminated:

$$
\begin{bmatrix}
1 & 2 & 2 & 2 & b_1 \\
2 & 4 & 6 & 8 & b_2 \\
3 & 6 & 8 & 10 & b_3
\end{bmatrix}
\to
\begin{bmatrix}
1 & 2 & 2 & 2 & b_1 \\
0 & 0 & 2 & 4 & b_2 - 2b_1 \\
0 & 0 & 0 & 0 & b_3 - b_2 - b_1 \\
\end{bmatrix}
$$

If $A\mathbf{x} = \mathbf{b}$ has a solution, then $b_3 - b_2 - b_1 = 0$. For example, we could choose $\mathbf{b} = \begin{bmatrix} 1 \\ 5 \\ 6 \end{bmatrix}$.

From an earlier lecture, we know that $A\mathbf{x} = \mathbf{b}$ is solvable exactly when $\mathbf{b}$ is in the column space $C(A)$. We have these two conditions on $\mathbf{b}$; in fact they are equivalent.

## Complete solution

In order to find all solutions to $A\mathbf{x} = \mathbf{b}$ we first check that the equation is solvable, then find a particular solution. We get the complete solution of the equation by adding the particular solution to all the vectors in the nullspace.

### A particular solution

One way to find a particular solution to the equation $A\mathbf{x} = \mathbf{b}$ is to set all free variables to zero, then solve for the pivot variables.
For our example matrix $A$, we let $x_2 = x_4 = 0$ to get the system of equations:

$$
\begin{align}
x_1 + 2x_3 &= 1 \\
2x_3 &= 3 \\
\end{align}
$$

which has the solution $x_3 = 3/2$, $x_1 = -2$. Our particular solution is:

$$
x_p =
\begin{bmatrix}
-2 \\
0 \\
3/2 \\
0 \\
\end{bmatrix}
$$

### Combined with the nullspace

The general solution to $A\mathbf{x} = \mathbf{b}$ is given by $x_{complete} = x_p + x_n$, where $x_n$ is a generic vector in the nullspace. To see this, we add $Ax_p = \mathbf{b}$ to $Ax_n = \mathbf{0}$ and get $A(x_p + x_n) = \mathbf{b}$ for every vector $x_n$ in the nullspace.

Last lecture we learned that the nullspace of $A$ is the collection of all combinations of the special solutions $\begin{bmatrix} -2 \\ 1 \\ 0 \\ 0 \end{bmatrix}$ and $\begin{bmatrix} 2 \\ 0 \\ -2 \\ 1 \end{bmatrix}$. So the complete solution to the equation $A\mathbf{x} = \begin{bmatrix} 1 \\ 5 \\ 6\end{bmatrix}$ is:

$$
x_{complete} =
\begin{bmatrix}
-2 \\
0 \\
3/2 \\
0 \\
\end{bmatrix}
+ c_1
\begin{bmatrix}
-2 \\
1 \\
0 \\
0 \\
\end{bmatrix}
+ c_2
\begin{bmatrix}
2 \\
0 \\
-2 \\
1 \\
\end{bmatrix}
$$

where $c_1$ and $c_2$ are real numbers.

The nullspace of $A$ is a two dimensional subspace of $\mathbb{R}^4$, and the solutions to the equation $A\mathbf{x} = \mathbf{b}$ form a plane parallel to that through $x_p = \begin{bmatrix} -2 \\ 0 \\ 3/2 \\ 0 \end{bmatrix}$

## Rank

The rank of a matrix equals the number of pivots of that matrix. If $A$ is an $m$ by $n$ matrix of rand $r$, we know:

- $r \le m$
- $r \le n$

### Full column rank

If $r = n$, then from the previous lecture we know that the nullspace has dimension $n - r = 0$ and contains only the zero vector. There are no free variables or special solutions.

If $A\mathbf{x} = \mathbf{b}$ has solution, it is unique; there is either 0 or 1 solution. Examples like this, in which the columns are independent, are common in applications.

We know $r \le m$, so if $r = n$ the number of columns of the matrix is less than or equal to the number of rows. The row reduced echelon form of the matrix will look like $R = \begin{bmatrix} I \\ 0 \end{bmatrix}$. For any vector $\mathbf{b}$ in $\mathbb{R}^m$ that's not a linear combination of the columns of $A$, there is no solution to $A\mathbf{x} = \mathbf{b}$.

### Full row rank

If $r = m$, then the reduced matrix $R = \begin{bmatrix} I & F\end{bmatrix}$ has no rows of zeros and so there are no requirements for the entries of $\mathbf{b}$ to satisfy. The equation $A\mathbf{x} = \mathbf{b}$ is solvable for every $\mathbf{b}$. There are $n - r = n - m$ free variables, so there are $n - m$ special solutions to $A\mathbf{x} = \mathbf{b}$

### Full row and column rank

If $r = m = n$ is the number of pivots of $A$, then $A$ is an invertible square matrix and $R$ is the identity matrix. The nullspace has dimension zero, and $A\mathbf{x} = \mathbf{b}$ has a unique solution for every $\mathbf{b}$ in $\mathbb{R}^m$.

### Summary

If $R$ is in row reduced form with pivot columns first(rref), the table below summarizes our results.

|     | r = m = n | r = n < m | r = m < n | r < m, r < n |
|---|---|---|---|---|
| R | $I$ | \begin{bmatrix} I \\\\ 0 \end{bmatrix} | \begin{bmatrix} I & F \end{bmatrix} | \begin{bmatrix} I & F \\\\ 0 & 0 \end{bmatrix} |
| #solutions to $Ax = b$ | 1 | 0 or 1 | infinite | 0 or infinite |
