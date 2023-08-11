# Factorization into A=LU

One goal of today's lecture is to understand Gaussian elimination int terms of matrices; to find a matrix $L$ such that $A = LU$. We start with some useful facts about matrix multiplication.

## Inverse of a product

The inverse of a matrix product $AB$ is $B^{-1}A^{-1}$.

## Transpose of a product

We obtain the `transpose` of a matrix by exchanging its rows and columns. In other words, the entry in row $i$ column $j$ of $A$ is the entry in row $j$ column $i$ of $A^T$.

The transpose of a matrix product $AB$ is $B^TA^T$. For any invertible matrix $A$, the inverse of $A^T$ is $(A^{-1})^T$.

## A = LU

We've seen how to use elimination to convert a suitable matrix $A$ into upper triangular matrix $U$. This leads to the factorization $A = LU$, which is very helpful in understanding the matrix $A$.

Recall that (when there are no row exchanges) we can describe the elimination of the entries of matrix $A$ in terms of multiplication by a succession of elimination matrices $E_{ij}$, so that $A \to E_{21}A \to E_{31}E_{21}A \to \cdots \to U$. In the two by two case this looks like:

$$
E_{21} \quad A \quad = \quad U
$$

$$
\begin{bmatrix}
1 & 0 \\ -4 & 1
\end{bmatrix}
\begin{bmatrix}
2 & 1 \\ 8 & 7
\end{bmatrix}
=
\begin{bmatrix}
2 & 1 \\ 0 & 3
\end{bmatrix}
$$

We can convert this to a factorization $A = LU$ by "canceling" the matrix $E_{21}$; multiply by its inverse to get $E_{21}^{-1}E_{21}A = E_{21}^{-1}U$.

$$
A \quad = \quad L \quad U
$$

$$
\begin{bmatrix}
2 & 1 \\ 8 & 7
\end{bmatrix}
=
\begin{bmatrix}
1 & 0 \\ 4 & 1
\end{bmatrix}
\begin{bmatrix}
2 & 1 \\ 0 & 7
\end{bmatrix}
$$

The matrix $U$ is `upper triangular` with pivots on the diagonal. The matrix $L$ is `lower triangular` and has ones on the diagonal. Sometimes we will also want to factor out a diagonal matrix whose entries are the pivots:

$$
A \qquad = \qquad L \qquad D \qquad U'
$$

$$
\begin{bmatrix}
2 & 1 \\ 8 & 7
\end{bmatrix}
=
\begin{bmatrix}
1 & 0 \\ 4 & 1
\end{bmatrix}
\begin{bmatrix}
2 & 0 \\ 0 & 3
\end{bmatrix}
\begin{bmatrix}
1 & 1/2 \\ 0 & 1
\end{bmatrix}
$$

In the three dimensional case, if $E_{32}E_{31}E_{21}A = U$ then $A = E_{21}^{-1}E_{31}^{-1}E_{32}^{-1}U = LU$.

For example, suppose $E_{31}$ is the identity matrix and $E_{32}$ and $E_{21}$ are as shown below:

$$
E_{32} \qquad E_{21} \qquad = \qquad E
$$

$$
\begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
0 & -5 & 1
\end{bmatrix}
\begin{bmatrix}
1 & 0 & 0 \\
-2 & 1 & 0 \\
0 & 0 & 1
\end{bmatrix}
=
\begin{bmatrix}
1 & 0 & 0 \\
-2 & 1 & 0 \\
10 & -5 & 1
\end{bmatrix}
$$

The $10$ in the below left corner arises because we subtracted twice the first row from the second row from the second row, then subtracted five times the new second row from the third.

The factorization $A = LU$ is preferable to the statement $EA = U$ because the combination of row subtractions does not have the effect on $L$ that it did on $E$. Here $L = E^{-1} = E_{21}^{-1}E_{32}^{-1}$:

$$
E_{21}^{-1} \qquad E_{32}^{-1} \qquad = \qquad L
$$

$$
\begin{bmatrix}
1 & 0 & 0 \\
2 & 1 & 0 \\
0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 5 & 1
\end{bmatrix}
=
\begin{bmatrix}
1 & 0 & 0 \\
2 & 1 & 0 \\
0 & 5 & 1
\end{bmatrix}
$$

Notice the $0$ in row three column one of $L = E^{-1}$, where $E$ had a $10$. If there are no row exchanges, the multipliers from the elimination matrices are copied directly into $L$.

## How expensive is elimination?

Some application require inverting very large matrices. This is done using a computer, of course. How hard will the compulter have to work? How long will it take?

When using elimination to find the factorization $A = LU$ we just saw that we can build $L$ as we go by keeping track of row subtractions. We have to remember $L$ and (the matrix which will become) $U$; we don't have to store $A$ or $E_{ij}$ in the computer's memory.
How many operations does the computer perform during the elimination process for an $n \times n$ matrix? A typical operation is to multiply one row and then subtract it from another, which requires on the order of $n$ operations. There are $n$ rows, so the total number of operaations used in eliminating entries in the first column is about $n^2$. The second row and column are shorter; that product costs about $(n - 1)^2$ operations, and so on. The total number of operations needed to factor $A$ into $LU$ is on the order of $n^3$:

$$
1^2 + 2^2 + \cdots + (n - 1)^2 + n^2 = \sum_{i = 1}^n i^2 \approx \int_{0}^n x^2 dx = \frac{1}{3} n^3
$$

While we're factoring $A$ we're also operating on $\mathbf{b}$. That costs about $n^2$ operations, which is hardly worth couting compared to $\frac{1}{3}n^3$.

## Row exchanges

What if there are row exchanges? In other words, what happens if there's a zero in a pivot position?
To swap two rows, we multiply on the left by a permutation matrix. Fro example,

$$
P_{12} =
\begin{bmatrix}
0 & 1 & 0 \\
1 & 0 & 0 \\
0 & 0 & 1
\end{bmatrix}
$$

swaps the first and second rows of a $3 \times 3$ matrix. The inverse of any permu7tation matrix $P$ is $P^{-1} = P^T$.

There are $n!$ different ways to permute the rows of an $n \times n$ matrix (including the permutation that leaves all rows fixed) so there are $n!$ permutation matrices. These matrices form a `multiplicative group`.

