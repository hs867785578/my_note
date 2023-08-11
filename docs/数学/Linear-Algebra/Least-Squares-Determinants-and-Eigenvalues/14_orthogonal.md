# Orthogonal vectors and subspaces

In this lecture we learn what it means for vectors, bases and subspaces to be `orthogonal`. The symbol for this is $\bot$.

The `big picture` of this course is that the row space of a matrix is orthogonal to its nullspace, and its column space is orthogonal to its left nullspace.

![orthogonal](images/orthogonal/orthogonal.png)

## Orthogonal vectors

`Orthogonal` is just another word for `perpendicular`. Two vectors are `orthogonal` if the angle between them is 90 degrees. If two vectors are orthogonal, they form a right triangle whose hypotenuse is the sum of the vectors. Thus, we can use the `Pythagorean theorem` to prove that the `dot product` $\mathbf{x}^T\mathbf{y} = \mathbf{y}^T \mathbf{x}$ is $zero$ exactly when $\mathbf{x}$ and $\mathbf{y}$ are orthogonal. (The length squared $||\mathbf{x}||^2$ equals $\mathbf{x}^T\mathbf{x}$).

Note that all vectors are orthogonal to the zero vector.

## Orthogonal subspaces

Subspace $S$ is `orthogonal` to subspace $T$ means: every vector in $S$ is orthogonal to every vector in $T$. The blackboard is not orthogonal to the floor; two vectors in the line where the blackboard meets the floor aren't orthogonal to each other.

In the plane, the space containing only the zero vector and any line through the origin are orthogonal subspaces. A line through the origin and the whole plane are never orthogonal subspaces. Two lines through the origin are orthogonal subspaces if they meet at right angles.

## Nullspace is perpendicular to row space

The row space of a matrix is orthogonal to the nullspace, because the definition of nullspace is the subspace with all the $\mathbf{x}$s that meet $A\mathbf{x}= \mathbf{0}$, which means the dot product of $\mathbf{x}$ with each row of $A$ is $0$. Then the product of $\mathbf{x}$ with any combination of rows of $A$ must be $0$.

The column space is orthogonal to the left nullspace of $A$ because the row space of $A^T$ is perpendicular to the nullspace of $A^T$.

In some sense, the row space and the nullspace of a matrix subdivide $\mathbb{R}^n$ into two perpendicular subspaces. For $A=\begin{bmatrix} 1 & 2 & 5 \\ 2 & 4 & 10 \end{bmatrix}$, the row space has dimension $1$ and basis $\begin{bmatrix} 1 \\ 2 \\ 5 \end{bmatrix}$ and the nullspace has dimension $2$ and is the plane through the origin perpendicular to the vector $\begin{bmatrix} 1 \\ 2 \\ 5 \end{bmatrix}$.

Not only is the nullspace orthogonal to the row space, their dimensions add up to the dimension of the whole space. We say that the nullspace and the row space are `orthogonal complements` in $\mathbb{R}^n$. The nullspace contains all the vectors that are perpendicular to the row space, and vice versa.

We could say that this is part two of the fundamental theorem of linear algebra. Part one gives the dimensions of the four subspaces, part two says those subspaces come in orthogonal pairs, and part three will be about orthogonal bases for these subspaces.

## N(A^TA) = N(A)

Due to measurement error, $A\mathbf{x} = \mathbf{b}$ is often unsolvable if $m > n$. Our next challenge is to find the best possible solution in this case. The matrix $A^TA$ plays a key role in this effort: the central equation is

$$
A^TA \hat{\mathbf{x}} = A^T \mathbf{b}
$$

We know that $A^TA$ is square $n \times n$ and symmetric. When is it invertible?

Suppose $A = \begin{bmatrix} 1 & 1 \\ 1 & 2 \\ 1 & 5 \end{bmatrix}$. Then:

$$
A^T A =
\begin{bmatrix} 1 & 1 & 1 \\ 1 & 2 & 5 \end{bmatrix} \begin{bmatrix} 1 & 1 \\ 1 & 2 \\ 1 & 5 \end{bmatrix}
= \begin{bmatrix} 3 & 8 \\ 8 & 30 \end{bmatrix}
$$

is invertible. $A^TA$ is not always invertible. In fact:

$$
N(A^TA) = N(A)
$$

$$
\text{rank of } A^T A = \text{rank of } A
$$

We conclude that $A^TA$ is invertible exaclty when $A$ has independent columns.
