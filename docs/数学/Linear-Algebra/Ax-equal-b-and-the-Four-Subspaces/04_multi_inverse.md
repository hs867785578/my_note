# Multiplication and Inverse Matrices

## Matrix Multiplication
We discuss four different ways of thinking about the product of two matrices. 

$$
AB = C
$$

If $A$ is an $m \times n$ matrix and $B$ is an $n \times p$ matrix, then $C$ is an $m \times p$ matrix. We use $c_{ij}$ to denote the entry in row i and column j of matrix $C$.

### Standard (row times column)
The standard way of describing a matrix product is to say that $c_{ij}$ equals the dot product  of row i of matrix A and column j of matrix B:

$$
c_{ij} = \sum_{k = 1}^{n} a_{ik}b_{kj}
$$

### Columns
The product of matrix A and column j of matrix B equals column j of matrix C. This tells us that the columns of C are combinations of columns of A.

### Rows
The product of row i of matrix A and matrix B equals row i of matrix C. So the rows of C are combinations of rows of B.

### Column Times Row
A column of A is an $m \times 1$ vector and a row of B is a $1 \times p$ vector. Their product is a matrix:

$$
\begin{bmatrix} 2 \\ 3 \\ 4 \end{bmatrix} \begin{bmatrix} 1 & 6 \end{bmatrix} = \begin{bmatrix} 2 & 12 \\ 3 & 18 \\ 4 & 24 \end{bmatrix}
$$

The columns of this matrix are multiplies of the column of A and the rows are multiplies of the row of B. If we think of the entries in these rows as the coordinates $(2, 12)$ or $(3, 18)$ or $(4, 24)$, all these points lie on the same line; similarly for the two column vectors. Later we'll see that this is equaivalent to saying that the `row space` of this matrix is a single line, as is the `column space`.

The product of A and B is the sum of these "column times rows" matrices:

$$
AB = \sum_{k = 1}^{n} \begin{bmatrix} a_{1k} \\ \vdots \\ a_{mk}\end{bmatrix} \begin{bmatrix} b_{k1} & \cdots & b_{kn}\end{bmatrix}
$$

### Blocks
If we subdivide A and B into blocks that match properly, we can write the product $AB = C$ in terms of products of the blocks:

$$
\begin{bmatrix} A_1 & A_2 \\ A_3 & A_4 \end{bmatrix} \begin{bmatrix} B_1 & B_2 \\ B_3 & B_4 \end{bmatrix} =\begin{bmatrix} C_1 & C_2 \\ C_3 & C_4 \end{bmatrix}
$$

Here $C_1 = A_1B_1 + A_2B_3$

## Inverses

### Square Matrices
If $A$ is a square matrix, the most important question you can ask it is whether it has an inverse $A^{-1}$. If it does, then $A^{-1}A = I = AA^{-1}$ and we say that $A$ is `invertible` or `nonsingular`.

If $A$ is `singular`, its determinant is zero and we can find some non-zero vector $\mathbf{x}$ for which $A\mathbf{x} = 0$. For example:

$$
\begin{bmatrix} 1 & 3 \\ 2 & 6 \end{bmatrix} \begin{bmatrix} 3 \\ -1 \end{bmatrix} =  \begin{bmatrix} 0 \\ 0 \end{bmatrix}
$$

In this example, three times the column minus one times the second column equals the zero vector; the two column vectors lie on the same line.

Finding the inverse of a matrix is closely related to solving systems of linear equations:

$$
\begin{bmatrix} 1 & 3 \\ 2 & 7 \end{bmatrix} \begin{bmatrix} a & c \\ b & d \end{bmatrix} =  \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}
$$

$$
A A^{-1} = I
$$

can be read as saying "A times column j of $A^{-1}$ equals column j of the identity matrix". This is just a special form of the equation $A\mathbf{x} =\mathbf{b}$.

### Gauss-Jordan Elimination
We can use the method of elimination to solve two or more linear equations at the same time. Just augment the matrix with the whole identity matrix $I$:

$$
\begin{bmatrix} 1 & 3 & \vdots & 1 & 0 \\ 2 & 7 & \vdots & 0 & 1 \end{bmatrix} \to \begin{bmatrix} 1 & 3 & \vdots & 1 & 0 \\ 0 & 1 & \vdots & -2 & 1 \end{bmatrix} \to \begin{bmatrix} 1 & 0 & \vdots & 7 & -3 \\ 0 & 1 & \vdots & -2 & 1 \end{bmatrix}
$$

Once we have used Gauss's elimination method to convert the origin matrix to upper triangular form, we go on to use Jorden's idea of eliminating entries  in the upper right portion of the matrix.

$$
A^{-1} = \begin{bmatrix} 7 & -3 \\ -2 & 1 \end{bmatrix}
$$

As in the last lecture, we can write the results of the elimination method as the product of a number of elimination matrix $E_{ij}$ whith the matrix $A$. Letting $E$ be the product of all $E_{ij}$, we write the result of this Guass-Jorden elimination using block matrix $E [A | I] = [I | E]$. But if $EA = I$, then $E = A^{-1}$.
