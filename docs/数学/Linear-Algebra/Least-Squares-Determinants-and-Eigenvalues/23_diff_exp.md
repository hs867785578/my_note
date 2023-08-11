# Differential Equations and e^At

The system of equations below describes how the values of variables $u_{1}$ and $u_{2}$ affect each other over time:

$$
\begin{align}
\frac{du_{1}}{dt} = -u_{1} + 2 u_{2} \\
\frac{du_{2}}{dt} = u_{1} - 2 u_{2} \\
\end{align}
$$

Just as we applied linear algebra to solve a difference equation, we can use it to solve this differential equation. For example, the initial condition $u_{1} = 1$, $u_{2} = 0$ can be written $\mathbf{u}(0) = \begin{bmatrix} 1 \\ 0 \end{bmatrix}$.

## Differential Equation du/dt = Au

By looking at the equations above, we might guess that over time $u_{1}$ will decrease. We can get the same sort of information more safely by looking at the eigenvalues of the matrix $A = \begin{bmatrix} -1 & 2 \\ 1 & -2 \end{bmatrix}$ of our system $\frac{d\mathbf{u}}{dt} = A \mathbf{u}$. Because A is singular and its trace is -3, we know that its eigenvalues are $\lambda_1 = 0$ and $\lambda_2 = -3$. The solution will turn out to include $e^{-3t}$ and $e^{0t}$. As t increases, $e^{-3t}$ vanishes and $e^{0t} = 1$ remains constant. Eigenvalues equal to zero have eigenvectors that are `steady state` solutions.

$\mathbf{x_1} = \begin{bmatrix} 2 \\ 1\end{bmatrix}$ is an eigenvectors for which $A\mathbf{x_1} = 0 \mathbf{x_1}$. To find an eigenvector corresponding to $\lambda_2 = -3$ we solve $(A - \lambda_2 I) \mathbf{x_2} = \mathbf{0}$:

$$
\begin{align}
\begin{bmatrix} 2 & 2 \\ 1 & 1 \end{bmatrix} \mathbf{x_2} = 0 \\
so \\
\mathbf{x_2} = \begin{bmatrix} 1 \\ -1 \end{bmatrix} \\
\end{align}
$$

and we can check that $A\mathbf{x_2} = -3 \mathbf{x_2}$. The general solution to this system of differential equations will be:

$$
\mathbf{u}(t) = c_1 e^{\lambda_1 t} \mathbf{x_1} + c_2 e^{\lambda_2 t} \mathbf{x_2}
$$

Is $e^{\lambda_1 t} \mathbf{x_1}$ really a solution to $\frac{d\mathbf{u}}{dt} = A\mathbf{u}$? To find out, plug in $\mathbf{u} = e^{\lambda_1 t} \mathbf{x_1}$:

$$
\frac{d\mathbf{u}}{dt} = \lambda_1 e^{\lambda_1 t} \mathbf{x_1}
$$

which agrees with:

$$
A\mathbf{u} = e^{\lambda_1 t} A \mathbf{x_1} = \lambda_1 e^{\lambda_1 t} \mathbf{x_1}
$$

The two "pure" terms $e^{\lambda_1 t}\mathbf{x_1}$ and $e^{\lambda_2 t} \mathbf{x_2}$ are analogous to the terms $\lambda_i^k \mathbf{x_i}$ we saw in the solution $c_1 \lambda_1^k \mathbf{x_1} + c_2 \lambda_2^k \mathbf{x_2} + \cdots + c_n \lambda_n^k \mathbf{x_n}$ to the difference equation $\mathbf{u_{k = 1}} = A \mathbf{u_k}$.

Plugging in the values of the eigenvectors, we get:

$$
\mathbf{u}(t) = c_1 e^{\lambda_1 t} \mathbf{x_1} + c_2 e^{\lambda_2 t} \mathbf{x_2} = c_1 \begin{bmatrix} 2 \\ 1 \end{bmatrix} + c_2 e^{-3t} \begin{bmatrix} 1 \\ -1 \end{bmatrix}
$$

We know $\mathbf{u}(0) = \begin{bmatrix} 1 \\ 0 \end{bmatrix}$, so at $t = 0$:

$$
\begin{bmatrix} 1 \\ 0 \end{bmatrix} = c_1\begin{bmatrix} 2 \\ 1 \end{bmatrix} + c_2 \begin{bmatrix} 1 \\ -1 \end{bmatrix}.
$$

$c_1 = c_2 = 1/3$ and $\mathbf{u}(t) = \frac{1}{3} \begin{bmatrix} 2 \\ 1 \end{bmatrix} + \frac{1}{3} e^{-3t} \begin{bmatrix} 1 \\ -1 \end{bmatrix}$ 

This tells us that the system starts with $u_1 = 1$ and $u_2 = 0$ but that as $t$ approaches infinitely, $u_1$ decays to $2/3$ and $u_2$ increases to $1/3$. This might describe stuff moving from $u_1$ to $u_2$.

The steady state of this system is $\mathbf{u}(\infty) = \begin{bmatrix} 2/3 \\ 1/3\end{bmatrix}$.

### Stability

Not all systems have a steady state. The eigenvalues of A will tell us what sort of solutions to expect:

1. Stability: $\mathbf{u}(t) \to 0$ when $Re(\lambda) < 0$.
2. Steady state: One eigenvalue is 0 and all other eigenvalues have negtive real part.
3. Blow up: if $Re(\lambda) > 0$ for any eigenvalues $\lambda$.

If 2 by 2 matrix $A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$ has two eigenvalues with negative real part, its trace $a + d$ is negative, the converse is not true: $\begin{bmatrix} -2 & 0 \\ 0 & 1 \end{bmatrix}$ has negative trace but one of its eigenvalues is 1 and $e^{1t}$ blows up. If A has a positive determinant and negative trace then the corresponding solutions must be stable.

### Applying S

The final step of our solution to the system $\frac{d\mathbf{u}}{dt} = A \mathbf{u}$ was to solve:

$$
c_1 \begin{bmatrix} 2 \\ 1 \end{bmatrix} + c_2 \begin{bmatrix} 1 \\ -1 \end{bmatrix} = \begin{bmatrix} 1 \\ 0 \end{bmatrix}
$$

In matrix form:

$$
\begin{bmatrix}
2 & 1 \\ 1 & -1
\end{bmatrix}
\begin{bmatrix}
c_1 \\ c_2
\end{bmatrix}
=
\begin{bmatrix}
1 \\ 0
\end{bmatrix}
$$

or $S\mathbf{c} = \mathbf{u}(0)$, where S is the eigenvector matrix. The components of $\mathbf{c}$ determine the contribution from each pure exponential solution, based on the initial conditions of the system.

In the equation $\frac{d\mathbf{u}}{dt} = A\mathbf{u}$, the matrix A couples the pure solutions. We set $\mathbf{u} = S \mathbf{v}$, where S is the matrix of eigenvectors of A, to get:

$$
S\frac{d\mathbf{v}}{dt} = AS\mathbf{v}
$$

or:

$$
\frac{d\mathbf{v}}{dt} = S^{-1} AS \mathbf{v} = \Lambda \mathbf{v}
$$

This diagonalizes the system: $\frac{dv_i}{dt} = \lambda_i v_i$. The general solution is then:

$$
\begin{align}
\mathbf{v}(t) &= e^{\Lambda t}\mathbf{v}(0) \\
\mathbf{u}(t) &= Se^{\Lambda t}S^{-1} \mathbf{v}(0) = e^{At} \mathbf{u}(0)
\end{align}
$$

## Matrix Exponential e^At

What does $e^{At}$ mean  if A is a matrix? We know that for a real number x:

$$
e^x = \sum_{n = 0}^{\infty} \frac{x^n}{n!} = 1 + x + \frac{x^2}{2} + \frac{x^3}{6} + \cdots
$$

We can use the same formula to define $e^{At}$:

$$
e^{At} = I + At + \frac{(At)^2}{2} + \frac{(At)^3}{6} + \cdots
$$

Similarly, if the eigenvalues of $At$ are small, we can use the geometric series $\frac{1}{1 - x} = \sum_{n = 0}^{\infty} x^n$ to estimate $(I - At)^{-1} = I + At + (At)^2 + (At)^3 + \cdots$

We've said that $e^{At} = Se^{\Lambda t} S^{-1}$. If A has n independent eigenvectors we can prove this from the definition of $e^{At}$ by using the formula $A = S \Lambda S^{-1}$:

$$
\begin{align}
e^{At} = & I + At + \frac{(At)^2}{2} + \frac{(At)^3}{6} + \cdots  \\
&= SS^{-1} + S\Lambda S^{-1} t + \frac{S \Lambda^2S^{-1}}{2}t^2 + \frac{S \Lambda^3S^{-1}}{6}t^3 + \cdots \\
&= S e^{\Lambda t} S^{-1}
\end{align}
$$

It's impractical to add up infinitely many matrices. Fortunately, there is an easier way to comput $e^{\Lambda t}$. Remember that:

$$
\Lambda =
\begin{bmatrix}
\lambda_1 & 0 & \cdots & 0 \\
0 & \lambda_2 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & \cdots & 0 & \lambda_n \\
\end{bmatrix}
$$

When we plug this in to our formula for $e^{At}$ we find that:

$$
e^{\Lambda t} = 
\begin{bmatrix}
e^{\lambda_1 t} & 0 & \cdots & 0 \\
0 & e^{\lambda_2 t} \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & \cdots & 0 & e^{\lambda_n t}\\
\end{bmatrix}
$$

This is another way to see the relationship between the stability of $\mathbf{u}(t) = S e^{\Lambda t} S^{-1} \mathbf{v}(0)$ and the eigenvalues of A.

## Second order

We can change the second order equation $y'' + b y' + ky = 0$ into a 2 by 2 first order system using a method similar to the one we used to find a formula for the Fibonacci numbers. If $u = \begin{bmatrix} y' \\ y \end{bmatrix}$, then:

$$
u' = \begin{bmatrix} y'' \\ y' \end{bmatrix} = \begin{bmatrix} -b & -k \\ 1 & 0 \end{bmatrix} \begin{bmatrix} y' \\ y \end{bmatrix}
$$

We could use the methods we just learned to solve this system, and that would give us a solution to the second order scalar equation we started with.

If we start with a kth order equation we get a k by k matrix with coefficients of the equation in the first row and 1's on a diagonal below that; the rest of the entries are 0.
