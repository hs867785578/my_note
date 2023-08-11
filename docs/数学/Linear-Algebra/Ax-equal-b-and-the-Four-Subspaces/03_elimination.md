# Elimination with Matrices

## Method of Elimination
`Elimination` is the technique most commonly used by computer software to solve systems of linear equations. It finds a solution $\mathbf{x}$ to $A\mathbf{x} = \mathbf{b}$ whenever the matrix $A$ is invertible. In the example used in class:

$$
A = \begin{bmatrix} 1 & 2 & 1 \\ 3 & 8 & 1 \\ 0 & 4 & 1 \end{bmatrix}
$$

and

$$
\mathbf{b} = \begin{bmatrix} 2 \\ 12 \\ 2 \end{bmatrix}
$$

The steps are:

$$
A = \begin{bmatrix} 1 & 2 & 1 \\ 3 & 8 & 1 \\ 0 & 4 & 1 \end{bmatrix} \to \begin{bmatrix} 1 & 2 & 1 \\ 0 & 2 & -2 \\ 0 & 4 & 1 \end{bmatrix} \to U = \begin{bmatrix} 1 & 2 & 1 \\ 0 & 2 & -2 \\ 0 & 0 & 5 \end{bmatrix}
$$

The number $1$ in the upper left corner of $A$ is called the `first pivot`. We recopy the first row, then multiply the numbers in it by an appropriate value(in this case, 3) and substract those values from the numbers in the second row. The first number in the second row becomes $0$. We have thus `eliminated` the $3$ in row 2 and column 1.

The next step is to perform another elimination to get a $0$ in row 3 column 1; here this is already the case.

The `second pivot` is the value 2 which now appears in row 2 column 2. We find a multiplier (in this case 2) by which we multiply the second row to eliminate the 4 in row 3 column 2. The `third pivot` is then the 5 now in row 3 column 3.

We started with an invertible matrix $A$ and ended with an `upper triangular` matrix $U$; the lower left portion of $U$ is filled with zeros. Pivots $1, 2, 5$ are on the `diagonal` of $U$.

When calculating by hand, we can substract the vector $\mathbf{b} = \begin{bmatrix} 2 \\ 12 \\ 2 \end{bmatrix}$ efficiently by `augmenting` the matrxi $A$, appending the vector $\mathbf{b}$ as a fourth or final column:

$$
\begin{bmatrix} 1 & 2 & 1  & \vdots & 2 \\ 3 & 8 & 1 & \vdots & 12 \\ 0 & 4 & 1 & \vdots & 2 \end{bmatrix} \to \begin{bmatrix} 1 & 2 & 1 & \vdots & 2 \\ 0 & 2 & -2 & \vdots & 6 \\ 0 & 4 & 1 & \vdots & 2 \end{bmatrix} \to \begin{bmatrix} 1 & 2 & 1 & \vdots & 2 \\ 0 & 2 & -2 & \vdots & 6 \\ 0 & 0 & 5 & \vdots & -10 \end{bmatrix}
$$

Finally, we have $U = \begin{bmatrix} 1 & 2 & 1 \\ 0 & 2 & -2 \\ 0 & 0 & 5 \end{bmatrix}$ from $A$ and $\mathbf{c} = \begin{bmatrix} 2 \\ 6 \\ -10 \end{bmatrix}$ comes from $\mathbf{b}$.

The equation $U\mathbf{x} = \mathbf{c}$ is easy to solve by `back substitution`:

$$
\begin{align}
x + 2y + z = 2 \\
2y - 2z = 6 \\
5z = -10
\end{align}
$$

We have $z = -2, y = 1$ and $x = 2$. This is also the solution to the original system $A\mathbf{x} = \mathbf{b}$

The `determinant` of $U$ is the product of the pivots. We will see this again.

Pivots may not be $0$. If there is a zero in the pivot position, we must exchange that row with one below to get a non-zero value in the pivot position.

If there is a zero in the pivot position and no non-zero value below it, then the matrix $A$ is `not invertible`. Elimination can not be used to find a unique solution to this system of equations--it doesn't exist.

## Elimination Matrices
The product of a matrix(3x3) and a column vector(3x1) is a column vector(3x1) that is a linear combination of the columns of the matrix.

The product of a row(1x3) and a matrix(3x3) is a row(1x3) that is a linear combination of the rows of the matrix.

We can subtract 3 times row 1 of matrix A from row 2 of A by calculating the matrix product:

$$
\begin{bmatrix} 1 & 0 & 0 \\ -3 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 1 & 2 & 1 \\ 3 & 8 & 1 \\ 0 & 4 & 1 \end{bmatrix} = \begin{bmatrix} 1 & 2 & 1 \\ 0 & 2 & -2 \\ 0 & 4 & 1 \end{bmatrix}
$$

The `elimination matrix` used to eliminate the entry in row m column n is denoted $E_{mn}$. The calculation above took us from $A$ to $E_{21}A$. The three elimination steps leading to $U$ were $E_{32}(E_{31}(E_{21}A)) = U$, where $E_{31} = I$. Thus $E_{32}(E_{21}A) = U$.

Matrix multiplication is `associative`, so we can also write $(E_{32}E_{21})A = U$. The product $E_{32}E_{21}$ tells us how to get from $A$ to $U$. The `inverse` of matrix $E_{32}E_{21}$ tells us how to get from $U$ to $A$.

If we solve $U\mathbf{x} = EA\mathbf{x} = E\mathbf{b}$, then it is also true that $A\mathbf{x} = \mathbf{b}$. This is why the method of elimination works: all steps can be reversed.

A `permutation matrix` exchanges two rows of matrix:

$$
P = \begin{bmatrix} 0 & 1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix}
$$

The first and second rows of the matrix $PA$ are the second and first rows of the matrix $A$. The matrix $P$ is constructed by exchanging rows of the identity matrix.

To exchange the columns of a matrix, multiply on the right (as in $AP$) by a permutation matrix.
Note that matrix multiplication is not `commutative`: $PA \ne AP$.

## Inverse

We have a matrix:

$$
E_{21} = \begin{bmatrix} 1 & 0 & 0 \\ -3 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}
$$

which substracts 3 times row 1 from row 2. To "undo" this operation, we must add 3 times row 1 to row 2 using the inverse matrix:

$$
E_{21}^{-1} = \begin{bmatrix} 1 & 0 & 0 \\ 3 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}
$$

In fact, $E_{21}^{-1}E_{21} = I$
