# Determinant Formulas and Cofactors

Now that we know the properties of the determinant, it's time to learn some(rather messy) formulas for computing it.

## Formula for the Determinant

We know that the determinant has the following three properties:

1. $det I = 1$
2. Exchanging rows reverses the sign of the determinant.
3. The determinant is linear in each row separately.

Last class we listed seven consequences of these properties. We can use these ten properties to find a formula for the determinant of a 2 by 2 matrix:

$$
\begin{align}
\begin{vmatrix}
a & b \\ c & d
\end{vmatrix}
&= \begin{vmatrix} a & 0 \\ c & d\end{vmatrix} + \begin{vmatrix} 0 & b \\ c & d \end{vmatrix} \\ 
&= \begin{vmatrix} a & 0 \\ c & 0\end{vmatrix} + \begin{vmatrix} a & 0 \\ 0 & d \end{vmatrix} + \begin{vmatrix} 0 & b \\ c & 0 \end{vmatrix} + \begin{vmatrix} 0 & b \\ 0 & d \end{vmatrix} \\
&= 0 + ad + (-cb) + 0 \\
&= ad - bc
\end{align}
$$

By applying property 3 to separate the individual entries of each row we could get a formula for any other square matrix. However, for a 3 by 3 matrix we'll have to add the determinants of 27 different matrices! Many of those determinants are zero. The none-zero pieces are:

$$
\begin{align}
\begin{vmatrix}
a_{11} & a_{12} & a_{13} \\
a_{21} & a_{22} & a_{23} \\
a_{31} & a_{32} & a_{33} \\
\end{vmatrix}
& =
\begin{vmatrix}
a_{11} & 0 & 0 \\
0 & a_{22} & 0 \\
0 & 0 & a_{33} \\
\end{vmatrix}
+
\begin{vmatrix}
a_{11} & 0 & 0 \\
0 & 0 & a_{23} \\
0 & a_{32} & 0 \\
\end{vmatrix}
+
\begin{vmatrix}
0 & a_{12} & 0 \\
a_{21} & 0 & 0 \\
0 & 0 & a_{33} \\
\end{vmatrix}
+
\begin{vmatrix}
0 & a_{12} & 0 \\
0 & 0 & a_{23} \\
a_{31} & 0 & 0 \\
\end{vmatrix}
+
\begin{vmatrix}
0 & 0 & a_{13} \\
0 & a_{22} & 0 \\
0 & a_{32} & 0 \\
\end{vmatrix}
+
\begin{vmatrix}
0 & 0 & a_{13} \\
0 & a_{22} & 0 \\
a_{31} & 0 & 0 \\
\end{vmatrix} \\
&= a_{11}a_{22}a_{33} - a_{11}a_{23}a_{32} - a_{12}a_{21}a_{33} + a_{12}a_{23}a_{31} + a_{13}a_{21}a_{32} - a_{13}a_{22}a_{31}
\end{align}
$$

Each of the non-zero pieces has one entry form each row in each column, as in a permutation matrix, Since the determinant of a permutation matrix is either 1 or -1, we can again use the property 3 to find the determinants of each of these summands and obtain our formula.

One way to remember this formula is that the positive terms are products of entries going down and to the right in our original matrix, and the negtive terms are products going down and to the left. This rule of thumb doesn't work for matrices larger than 3 by 3.

The number of parts with non-zero determinants was

- 2($2!$), in 2 by 2 case,
- 6($3!$), in 3 by 3 case,
- 24($4!$), in 4 by 4 case.

This  is because there are n ways to choose an element from the first row(i.e. a value for $\alpha$), after which there are only n - 1 ways to choose an element from the second row that avoids a zero determinant. Then there are n - 2 choices from the third row, n - 3 from the fourth, and so on.

The big formula for computing the determinant of any square matrix is:

$$
det A = \sum_{n!\text{ terms}} \pm a_{1\alpha} a_{2\beta} a_{3\gamma} \cdots a_{n\omega}
$$

where $(\alpha,\beta,\gamma,\cdots,\omega)$ is some permutation of $(1, 2, 3, \cdots, n)$. If we test this on the identity matrix, we find that all the terms are zero except the one corresponding to the trivial permutation $\alpha=1, \beta = 2, \cdots, \omega = n$. This agrees with the first property: $det I = 1$. It's possible to check all the other properties as well, but we won't do that here.

Applying the method of elimination and multiplying the diagonal entries of the result (the pivots) is another good way to find the determinant of a matrix.

## Example

In a matrix with many zero entries, many terms in the formula are zero. We can compute the determinant of:

$$
\begin{vmatrix}
0 & 0 & 1 & 1 \\
0 & 1 & 1 & 0 \\
1 & 1 & 0 & 0 \\
1 & 0 & 0 & 1 \\
\end{vmatrix}
$$

by choosing a non-zero entry from each row and column, multiplying those entries, giving the product the appropriate sign, then adding the results.

The permutation corresponding to the diagonal running from $a_{14}$ to $a_{41}$ is (4, 3, 2, 1). This contributes 1 to the determinant of the matrix; the contribution is positive because it takes two row exchanges to convert the permutation (4, 3, 2, 1) to the identity (1, 2, 3, 4).

Another non-zero term of of $\sum \pm a_{1\alpha} a_{2\beta} a_{3 \gamma} a_{4 \omega}$ comes from the permutation (3, 2, 1, 4). This contributes -1 to the sum, because one exchange (of the first and third rows) leads to the identity.

These are the only two non-zero terms in the sum, so the determinant is 0. We can confirm this by noting that row 1 minus row 2 plus row 3 minus row 4 equals zero.

## Cofactor formula

The cofactor formula rewrites the big formula for the determinant of an n by n matrix in terms of the determinants of smaller matrices.

In the 3 by 3 case, the formula looks like:

$$
\begin{align}
det A &= a_{11}(a_{22}a_{33} - a_{23}a_{32}) + a_{12}(-a_{21}a_{33} + a_{23}a_{31}) + a_{13}(a_{21}a_{32} - a_{22}a_{31})
&= \begin{vmatrix} a_{11} & 0 & 0 \\ 0 & a_{22} & a_{23} \\ 0 & a_{32} & a_{33} \end{vmatrix} + \begin{vmatrix} 0 & a_{12} & 0 \\ a_{21} & 0 & a_{23} \\ a_{31} & 0 & a_{33} \end{vmatrix} + \begin{vmatrix} 0 & 0 & a_{13} \\ a_{21} & a_{22} & 0 \\ a_{31} & a_{32} & 0 \end{vmatrix}
\end{align}
$$

This comes from grouping all the multiples of $a_{ij}$ in the big formula. Each element is multiplied by the `cofactors` in the parentheses following it. Note that each cofactor is (plus or minus) the determinant of a 2 by 2 matrix. That determinant is made up of products of elements in the rows and columns NOT containing $a_{1j}$.

In general, the cofactor $C_{ij}$ of $a_{ij}$ can be found by looking at all the terms in the big formula that contain $a_{ij}$. $C_{ij}$ equals $(-1)^{i + j}$ times the determinant of the n - 1 by n - 1 square matrix obtained by removing row i and column j. ($C_{ij}$ is positive if i + j is even and negtive if i + j is odd.)

For n by n matrices, the cofactor formula is:

$$
det A = a_{11} C_{11} + a_{12} C_{12} + \cdots + a_{1n} C_{1n}
$$

Applying this to a 2 by 2 matrix gives us:

$$
\begin{vmatrix}
a & b \\ c & d
\end{vmatrix}
=
ad + b(-c)
$$

## Tridiagonal Matrix

A `tridiagonal matrix` is one for which the only non-zero entries lie on or adjacent to the diagonal. For example, the 4 by 4 triagonal matrix of 1's is:

$$
A_{4} = \begin{vmatrix} 1 & 1 & 0 & 0 \\ 1 & 1 & 1 & 0 \\ 0 & 1 & 1 & 1 \\ 0 & 0 & 1 & 1 \end{vmatrix}
$$

What is the determinant of an n by n triagonal matrix of 1's?

$$
\begin{vmatrix} A_{1} \end{vmatrix} = 1, 
\begin{vmatrix} A_{2} \end{vmatrix} = \begin{vmatrix} 1 & 1 \\ 1 & 1 \end{vmatrix} = 0,
\begin{vmatrix} A_{3} \end{vmatrix} = \begin{vmatrix} 1 & 1 & 0 \\ 1 & 1 & 1 \\ 0 & 1 & 1 \end{vmatrix} = -1
$$

$$
\begin{vmatrix} A_{4} \end{vmatrix} = 1 \begin{vmatrix} 1 & 1 & 0 \\ 1 & 1 & 1 \\ 0 & 1 & 1 \end{vmatrix} - \begin{vmatrix} 1 & 1 & 0 \\ 0 & 1 & 1 \\ 0 & 1 & 1 \end{vmatrix} = |A_{3}| - 1 |A_{2}| = -1
$$

In fact, $|A_{n}| = |A_{n - 1}| - |A_{n - 2}|$. We get a sequence which repeats every six terms:

$$
|A_1| = 1, |A_2| = 0, |A_3| = -1, |A_4| = -1, |A_5| = 0, |A_6| = 1, |A_7| = 1
$$
