# Linear Transformations and their Matrices

In older linear algebra courses, linear transformations were introduced before matrices. This geometric approach to linear algebra initially avoids the need for coordinates. But eventually there must be coordinates and matrices when the need for computation arises.

## Without Coordinates(no matrix)

### Example 1: Projection

We can describe a projection as a `linear transformation` T which takes every vector in $\mathbb{R}^2$ into another vector in $\mathbb{R}^2$. In other words,

$$
T : \mathbb{R}^2 \to \mathbb{R}^2
$$

The rule for this `mapping` is that every vector $\mathbf{v}$ is projected onto a vector $T(\mathbf{v})$ on the line of the projection. Projection is a linear transformation.

### Definition of Linear

A transformation T is `linear` if:

$$
T(\mathbf{v} + \mathbf{w}) = T(\mathbf{v}) + T(\mathbf{w})
$$

and

$$
T(c\mathbf{v}) = cT(\mathbf{v})
$$

for all vectors $\mathbf{v}$ and $\mathbf{w}$ for all scalars c. Equaivalently:

$$
T(c\mathbf{v} + d\mathbf{w}) = cT(\mathbf{v}) + dT(\mathbf{w})
$$

for all vectors $\mathbf{v}$ and $\mathbf{w}$ and scalars c and d. It's worth noticing that $T(\mathbf{0}) = \mathbf{0}$, because if not it couldn't be true that $T(c\mathbf{0}) = cT(\mathbf{0})$.

### Non-example 1: Shift the Whole Plane

Consider the transformation $T(\mathbf{v}) = \mathbf{v} + \mathbf{v}_ 0$ that shifts every vector in the plane by adding some fixed vector $\mathbf{v}_ 0$ to it. This is `not` a linear transformation because $T(2\mathbf{v}) = 2\mathbf{v} + \mathbf{v}_ 0 \ne 2T(\mathbf{v})$.

### Non-exmaple 2: T(v) = ||v||

The transformation $T(\mathbf{v}) = ||\mathbf{v}||$ that takes any vector to its length is not a linear transformation because $T(c\mathbf{v}) \ne cT(\mathbf{v})$ if $c < 0$.

We're not going to study transformations that aren't linear. From here on, we'll only use T to stand for linear transformations.

### Example 2: Rotation by 45 deg

This transformation $T : \mathbf{R}^2 \to \mathbf{R}^2$ takes vector $\mathbf{v}$ and outputs the vector $T(\mathbf{v})$ that comes from rotating $\mathbf{v}$ counterclockwise by $45^{\circ}$ about the origin. Note that we can describe this and see that it's linear without using any coordinates.

### The Big Picture

One advantage of describing transformations geometrically is that it helps us to see the big picture, as apposed to focusing on the transformation's effect on a single point. We can quickly see how rotation by $45^{\circ}$ will transform a picture of a house in the plane. If the transformation was described in terms of a matrix rather than as a rotation, it would be harder to guess what the house would be mapped to.

Frequently, the best way to understand a linear transformation is to find the matrix that lies behind the transformation. To do this, we have to choose a basis and bring in coordinates.

## With Coordinates(matrix)

All of the linear transformations we've discussed above can be described in terms of matrices. In a sense, linear transformations are an abstract description of multiplication by a matrix, as in the following example.

### Example 3: T(v) = Av

Given a matrix A, define $T(\mathbf{v}) = A\mathbf{v}$. This is a linear transformation:

$$
A(\mathbf{v} + \mathbf{w}) = A(\mathbf{v}) + A(\mathbf{w})
$$

and

$$
A(c\mathbf{v}) = cA(\mathbf{v})
$$

### Example 4

Suppose $A = \begin{bmatrix}1 & 0 \\ 0 & -1\end{bmatrix}$. How would we describe the transformation $T(\mathbf{v}) = A\mathbf{v}$ geometrically?

When we multiply A by a vector $\mathbf{v}$ in $\mathbb{R}^2$, the x component of the vector is unchanged and the sign of the y component of the vector is reversed. The transformation $\mathbf{v} \to A\mathbf{v}$ reflects the xy-plane across the x axis.

### Example 5

How could we find a linear transformation $T : \mathbb{R}^3 \to \mathbb{R}^2$ that takes three dimensional space to two dimensional space? Choose any 2 by 3 matrix A and define $T(\mathbf{v}) = A\mathbf{v}$.

### Describing T(v)

How much information do we need about T to determine $T(\mathbf{v})$ for all $\mathbf{v}$? If we know how T transforms a single vector $\mathbf{v}_ 1$, we can use the fact that T is a linear transformation to calculate $T(c\mathbf{v}_ 1)$ for any scalar c. If we know $T(\mathbf{v}_ 1)$ and $T(\mathbf{v}_ 2)$ for two independent vectors $\mathbf{v}_ 1$ and $\mathbf{v}_ 2$, we can predict how T will transform any vector $c\mathbf{v}_ 1 + d \mathbf{v}_ 2$ in the plan spanned by $\mathbf{v}_ 1$ and $\mathbf{v}_ 2$. If we wish to know $T(\mathbf{v})$ for all vectors $\mathbf{v}$ in $\mathbb{R}^n$, we just need to know $T(\mathbf{v}_ 1), T(\mathbf{v}_ 2), \cdots, T(\mathbf{v}_ n)$ for any basis $\mathbf{v}_ 1, \mathbf{v}_ 2, \cdots, \mathbf{v}_ n$ of the intput space. This is because any $\mathbf{v}$ in the input space can be written as a linear combination of basis vectors, and we know that T is linear:

$$
\mathbf{v} = c_1 \mathbf{v}_ 1 + c_2 \mathbf{v}_ 2 + \cdots + c_n \mathbf{v}_ n
$$

$$
T(\mathbf{v}) = c_1 T(\mathbf{v}_ 1) + c_2 T(\mathbf{v}_ n) + \cdots + c_n T(\mathbf{v}_ n)
$$

This is how we get from a (coordinate-free) linear transformation to a (coordinate based) matrix; the $c_i$ are our coordinates. Once we've chosen a basis, every vector $\mathbf{v}$ in the space can be written as a combination of basis vectors in exactly one way. The coefficients of those vectors are the `coordinates` of $\mathbf{v}$ in that basis.

Coordinates come from a basis; changing the basis changes the coordinates of vectors in the space. We may not use the standard basis all the time - we sometimes want to use a basis of eigenvectors or some other basis

### The Matrix of a Linear Transformation

Given a linear transformation T, how do we construct a matrix A that represents it?

First, we have to choose two bases:

- $\mathbf{v}_ 1, \mathbf{v}_ 2, \cdots, \mathbf{v}_ n$ of $\mathbb{R}^n$ to give coordinates to the input vectors;
- $\mathbf{w}_ 1, \mathbf{w}_ 2, \cdots, \mathbf{w}_ m$ of $\mathbb{R}^m$ to give coordinates to the output vectors.

We want to find a matrix A so that $T(\mathbf{v}) = A\mathbf{v}$, where $\mathbf{v}$ and $A\mathbf{v}$ get their coordinates from these bases.

The first column of A consists of the coefficients $a_{11}, a_{21}, \cdots, a_{1m}$ of $T(\mathbf{v}_ 1) = a_{11}\mathbf{w}_ 1 + a_{21}\mathbf{w}_ 2 + \cdots + a_{1m}\mathbf{w}_ m$. The entries of column i of the matrix A are determined by $T(\mathbf{v}_ i) = a_{1i} \mathbf{w}_ 1 + a_{2i} \mathbf{w}_ 2 + \cdots + a_{mi} \mathbf{w}_ m$. Because we've guaranteed that $T(\mathbf{v}_ i) = A\mathbf{v}_ i$ for each basis vector $\mathbf{v}_ i$ and because T is linear, we know that $T(\mathbf{v}) = A\mathbf{v}$ for all vectors $\mathbf{v}$ in the input space.

In the example of the projection matrix, $n = m = 2$. The transformation T projects every vector in the plane onto a line. In this example, it makes sense to use the same basis for the input and the output. To make our calculations as simple as possible, we'll choose $\mathbf{v}_ 1$ to be a unit vector on the line of projection and $\mathbf{v}_ 2$ to be a unit vector perpendicular to $\mathbf{v}_ 1$. Then:

$$
T(c_1 \mathbf{v}_ 1 + c_2 \mathbf{v}_ 2) = c_1 \mathbf{v}_ 1 + \mathbf{0}
$$

and the matrix of the projection transformation is just $A = \begin{bmatrix}1 & 0 \\ 0 & 0\end{bmatrix}$.

$$
A\mathbf{v} = \begin{bmatrix}1 & 0 \\ 0 & 0 \end{bmatrix} \begin{bmatrix}c_1 \\ c_2 \end{bmatrix} = \begin{bmatrix}c_1 \\ 0\end{bmatrix}
$$

This is a nice matrix! If our chosen basis consists of eigenvectors then the matrix of the transformation will be the diagonal matrix $\Lambda$ with eigenvalues on the diagonal.

To see how important the choice of basis is, let's use the standard basis for the linear transformation that projects the plane onto a line at a $45^{\circ}$ angle. If we choose $\mathbf{v}_ 1 = \mathbf{w}_ 1 = \begin{bmatrix}1 \\ 0\end{bmatrix}$ and $\mathbf{v}_ 2 = \mathbf{w}_ 2 = \begin{bmatrix}0 \\ 1\end{bmatrix}$, we get the projection matrix $P = \frac{\mathbf{a}\mathbf{a}^T}{\mathbf{a}^T\mathbf{a}} = \begin{bmatrix}1/2 & 1/2 \\ 1/2 & 1/2\end{bmatrix}$. We can check by graphing that this is the correct matrix, but calculating P directly is more difficult for this basis than it was with a basis of eigenvectors.

### Example 6: T = d / dx

Let T be a transformation that takes the derivative:

$$
T(c_1 + c_2x + c_3 x^2) = c_2 + 2 c_3 x
$$

The input space is the three dimensional space of quadratic polynomials $c_1 + c_2 x + c_3 x^2$ with basis $\mathbf{v}_ 1 = 1, \mathbf{v}_ 2 = x, \mathbf{v}_ 3 = x^2$. The output space is a two dimensional subspace of the input space with $\mathbf{w}_ 1 = \mathbf{v}_ 1 = 1$ and $\mathbf{w}_ 2 = \mathbf{v}_ 2 = x$.

This is a linear transformation! So we can find $A = \begin{bmatrix}0 & 1 & 0 \\ 0 & 0 & 2\end{bmatrix}$ and write the transformation as a matrix multiplication:

$$
T(\begin{bmatrix}c_1 \\ c_2 \\ c_3 \end{bmatrix}) = A\begin{bmatrix}c_1 \\ c_2 \\ c_3 \end{bmatrix} = \begin{bmatrix}c_2 \\ 2c_3\end{bmatrix}
$$

## Conclusion

For any linear transformation T we can find a matrix A so that $T(\mathbf{v}) = A\mathbf{v}$. If the transformation is invertible, the inverse transformation has the matrix $A^{-1}$. The product of two transformations $T_1 : \mathbf{v} \to A_1 \mathbf{v}$ and $T_2 : \mathbf{w} \to A_2 \mathbf{w}$ corresponds to the product $A_2A_1$ of their matrices. This is where matrix multiplication came from!
