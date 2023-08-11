# Optimization and Linearization
Basically, optimization involves finding the best situation possible, wether that be the cheapest way to build a bridge without falling down or something as mundane as finding the fastest driving route to a specific destination.

Linearization is a useful technique for finding approximate values of hard-to-calculate quantities.

## Optimization

To "optimize" something means to make it as good as possible. The term "optimize" just means "maximize or minimize, as appropriate."

In general, here's the way to tackle optimization problems:

1. Identify all the variables you might possibly need. One of them should be the quantity you want to maximize or minimize--make sure you know which one! Let's call it Q for now, although of course it might be another letter like P, m, or $\alpha$.
2. Get a fell for the extremes of the situation, seeing how far you can push your variables.
3. Write down equations relating the variables. One of them should be an equation for Q.
4. Try to make Q a function of only one variable, using all your equations to eliminate the other variables.
5. Differentiate Q with respect to that variable, then find the critical points; remember, these occur where the derivative if 0 or $DNE$.
6. Find the values of Q at all the critical points and at the endpoints. Pick out the maximum and minimum values. As a verification, use a table of signs or the sign of the second derivative to classify the critical points.
7. Write out a summary of what you've found, identifying the variables in words rather than symbols(wherever possible).

## Linearization

The general method is to use the formula:

$$
f(a + \Delta x) \cong f(a) + f'(x)\Delta x
$$

For example, you want to find the value of $\sqrt{11}$, let

- $f(x) = \sqrt{x}$,

then:

- $f'(x) = \frac{1}{2\sqrt{x}}$,
- $\Delta x = 2$,
- $a = 3$,

We get:

$$
\sqrt{11} = f(9 + 2) \cong f(9) + f'(9)\Delta x = 3 + \frac{1}{3}
$$

Here is the graph of the situation:

![linear](images/linear/linear.png)

The graph shows the curve $y = f(x)$ and the linearization $y = L(x)$, which is the tangent line to the curve at $x = a$. We want to estimate the value of $f(a + \Delta x)$. That's the height of the point $F$ in the above picture. As an approximate value, we're actually using $L(x + \Delta x)$, which is the height of $P$ in the picture. The difference between the two quantities is labeled "error".

Since $L(a + \Delta x) = f(a) + f'(a) \Delta x$, we see that:

$$
df = f'(a) \Delta x
$$

The quantities $df$ is called the *differential of $f$ at $x = a$*.

We set:

$$
r(x) = f(x) - L(x)
$$

where $r(x)$ is the error in using the linearization at $x = a$ in order to estimate $f(x)$. It turns out that if the second derivative of $f$ exists, at least between x and a, then there's a nice formula for $r(x)$:

$$
r(x) = \frac{1}{2} f''(x)(x - a)^2
$$

The above formula is related to the Mean Value Theorem.

- if $f'' > 0$ between a and x, then using the linearization leads to an **underestimate**($L(x) < f(x)$).
- if $f'' < 0$ between a and x, then using the linearization leads to an **overestimate**($L(x) > f(x)$).

If we find the maximum M in $[a, x]$, we get the folowing:

$$
|error| \le \frac{1}{2} M |x - a|^2
$$

In summary, the basic strategy for estimating, or approximating, anasty number:

1. Write down the main formula:

$$
f(x) \cong L(x) = f(a) + f'(a)(x - a)
$$

2. Choose a function $f$, and a number $x$ such that the nasty number is equal to $f(x)$. Also, choose $a$ to $x$ such that $f(a)$ can easily be computed.
3. Differentiate $f$ to find $f'$
4. In the above formula, replace $f$ and $f'$ by the actual functions, and $a$ by the actual number you've chosen.
5. Finally, plug in the value of $x$ from step 2 above. Also note that the differential $df$ is the quantity $f'(a)(x - a)$

## Newton's Method

Here's another useful application of linearization. Suppose that you have an equation of the form $f(x) = 0$ that you'd like to solve, but you just can't solve the darned thing. So you do the next best thing: you take a guess at a solution, which you can call $a$. The situation might look like this:


