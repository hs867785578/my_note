# Tylor Polynomials, Tylor Series and Power Series

## Approximations and Taylor Polynomials
Here's a nice fact: for any real number *x*, we have:

$$
e^x \cong 1 + x + \frac{x^2}{2} + \frac{x^3}{6}
$$

Also, the closer *x* is to *0*, the better the approximation.

Let's say we have some function $f$ which is very smooth, so that it can be repeatedly differentiated as many times as you like without causing any problems. Here is the question: what is the equation of the line which best approximates the curve $y = f(x)$ near the point $(a, f(a))$? The answer to this question is that the line we're looking for is the tangent line to the curve at the point $(a, f(a))$:

$$
y = f(a) + f'(a)(x - a)
$$

![linear](images/taylor/linear.png)

Why stick to lines? What is the equation of the quadratic which best approximates the curve $y = f(x)$ near $(a, f(a))$? It turns out that the formula for the quadratic which best approximates the curve $y = f(x)$ for $x$ near $a$ is given by:

$$
y = f(a) + f'(a)(x - a) + \frac{f''(a)}{2} (x - a)^2
$$

This is actually a quadratic in $x$, because if you expand $(x -a)^2$, the highest power of $x$ floating around is $x^2$

Let's call the quadratic $P_2$:

$$
P_2(x) = f(a) + f'(a) (x - a) + \frac{f''(a)}{2} (x -  a)^2
$$

Let's continue the same pattern, except that we'll use some arbitrary degree $N$ instead of just 1 or 2. So, here's our question: which polynomial of degree $N$ or less gives the best approximation to $f(x)$ for values of $x$ near $a$? The answer is given by **A Tylor approximation theorem**:

> If $f$ is smooth at $x = a$, then of all the polynomials of degree $N$ or less, the one which best approximates $f(x)$ for $x$ near $a$ is given by:

$$
P_N(x) = f(a) + f'(a)(x - a) + \frac{f''(a)}{2!}(x - a)^2 + \frac{f^{(3)}(a)}{3!} (x - a)^3 + \cdots + \frac{f^{(n)}(a)}{N!} (x - a)^N
$$

In sigma notation, the formula looks like this:

$$
P_N(x) = \sum_{n = 0}^{N} \frac{f^{(n)}(a)}{n!} (x - a)^n
$$

We call the polynomial $P_N$ `the Nth-order Taylor polynomial of $f(x)$`

Now we have a approximation for $f(x)$, but how good is it? Here comes another formula for the error, the `Taylor' Theorem`:

In above contents we have a function's Nth-order Taylor polynomial about $x = a$:

$$
P_N(x) = \sum_{n = 0}^{N} \frac{f^{(n)}(a)}{n!} (x - a)^n
$$

We want to use the value of $P_N(x)$ to approximate the true value $f(x)$, so we consider the error term, which is the difference between the true value and the approximate value:

$$
R_N(x) = f(x) - P_N(x)
$$

Actually, $R_N(x)$ is called the `Nth-order term`, it's also referred to as the `Nth-order remainder term`. Tylor's Theorem gives an alternative formula for $R_N(x)$:

$$
R_N(x) = \frac{f^{(N + 1)}(c)}{(N + 1)!} (x - a)^{N + 1}
$$

where $c$ is some number which lies between $x$ and $a$.

Since $f(x) = P_N(x) + R_N(x)$, we can write:

$$
f(x) = \sum_{n = 0}^{N} \frac{f^{(n)}(a)} {n!} (x - a)^n + \frac{f^{(N + 1)}(c)} {(N + 1)!} (x - a)^{N + 1}
$$
