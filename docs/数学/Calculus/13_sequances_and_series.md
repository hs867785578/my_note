# Sequences and Series

Infinite series are pretty similar to improper integrals.

## Convergence and Divergence of Sequences

A **sequence** is a collection of numbers in order. It might have a finite number of terms, or it might go on forever, in which case it is called an `infinite sequence`. For example:

$$
0, 1, 2, 3, 4, ...
$$

Sequences are normally written using subscript notation, where $a_1$ denotes the first element of the series, $a_2$ the second, $a_3$ the third, and so on.

Given an infinite sequence, our main focus is going to be on the limiting behavior of the values of the sequence as the index $n$ tends to infinity. That is, what happens to the sequence as you look farther and farther along it? In math notation, does

$$
\lim_{n \to \infty} a_n
$$

exist, and if so, what is it?

The basic idea is that the statement

$$
\lim_{n \to \infty} a_n = L
$$

means that $a_n$ might wander around for a little while, but eventually gets very close to $L$ and stays at least as close to $L$ for ever after. If there's such a number $L$, then the sequence $\{a_n\}$ `converges`; otherwise it `diverges`. Just like functions, sequences can diverge to $\infty$ or $-\infty$, or they can oscillate around and not get close to any particular value.

### Two important sequences
Pick some constant number $r$ and consider the sequence given by $a_n = r^n$ starting at $n = 0$. This is a `geometric progression`.

These are all special cases of the general rule, which is as follows:

$$
\lim_{n \to \infty} r^n = 
\begin{cases}
= 0 & if \quad -1 < r < 1, \\
= 1 & if \quad r = 1, \\
= \infty & if \quad r > 1, \\
= DNE & if \quad r \le -1.
\end{cases}
$$

In particular, if $k$ is any constant, then

$$
\lim_{n \to \infty} (1 + \frac{k}{n})^n = e^k
$$

## Convergence and Divergence of Series

A series is just a sum. We'd like to add up all of the terms of a sequence $a_n$.
Let's define a new sequence, which we'll call $\{A_N\}$, by setting:

$$
A_N = a_1 + a_2 + a_3 + \cdots + a_{N - 1} + a_{N}
$$

The new sequence is called the sequence of `partial sums`. The weird equaton now looks like this:

$$
a_1 + a_2 + a_3 + \cdots = \lim_{N \to \infty} A_N
$$

If the limit exists and equals $L$, then we'll say that the seriess on the left-hand side `converges` to $L$. If the limit doesn't exist, then the series `diverges`.

The formula for $A_N$ is:

$$
A_N = a_1 + a_2 + a_3 + \cdots + a_{N - 1} + a_N = \sum_{n = 1}^N a_n
$$

The infinite seriess is written as:

$$
a_1 + a_2 + a_3 + \cdots = \sum_{n = 1} ^{\infty} a_n
$$

So, here's how to define the value of an infinite series using sigma notation:

$$
\sum_{n = 1} ^{\infty} a_n = \lim_{N \to \infty} \sum_{n = 1} ^N a_n
$$

### Geometric series(theory)

Let's look at an important example of an infinite series. Suppose we start with the geometric progression $1, r, r^2, r^3, \cdots$, we can use this sequence as the terms of an infinit series:

$$
1 + r + r^2 + r^3 + \cdots = \sum_{n = 0} ^{\infty} r^n
$$

This is called a `geometric series`. It turns out that the geometric series:

$$
\sum_{n = 0}^{\infty} r^n = \frac{1}{1 - r}
$$

- if $-1 < r < 1$;
- otherwise, if $r \ge 1$ or $r \le -1$, the series diverges.

## The n-th Term Test(Theory)

For a series to converge, the sequence of partial sums has to have a limit. The partial sum after N steps represents your position after you have taken N steps. If you  finally converge to some special limiting position as you keep on taking more and more steps, then your steps have to become really really small.

So your step sizes, which are just given by the sequence $\{a_n\}$, eventually have to become very small, at least if you want your series to converge. This leads to the `n-th term` test:

> if $\lim_{n \to \infty} a_n != 0$, or the limit doesn't exist, then the seriess $\sum_{n = 1}^{\infty} a_n$ diverges.

Just beware:
> the n-th term test cannot be used to show that a series converges!

## The p-test(Theory)
The p-test is basically the same as the p-test for improper integrals with problem spot at $\infty$:

$$
\sum_{n = a}^{\infty} \frac{1}{n^p}
\begin{cases}
converges & if \quad p > 1, \\
diverges & if \quad p \le 1.
\end{cases}
$$

## New Tests for Series
The tests above are from improper integral. Here are four tests for convergence of series which have no corresponding improper integral versions.

### The ratio test
To test the series $\sum_{n=1}^{\infty}a_n$, let's consider a new sequence $b_n$, and 

$$
b_n = |\frac{a_{n + 1}}{a_n}|
$$

The the series $\{\sum_{n=1}^{\infty}a_n\}$:

- converges, if the sequence ${b_n}$ converges to a number less than $1$;
- diverges, if the sequence ${b_n}$ converges to a number greater than $1$;

### The root test

The root test(also called the n-th root test) is a close cousin of the ratio test. Instead of considering ratios of successive terms, just consider the n-th root of the absolute value of n-th term.

Starting with a series $\sum_{n = 1}^{\infty} a_n$, let's make a new sequence given by:

$$
b_n = |a_n|^{\frac{1}{n}}
$$

If the limit of sequence $\{b_n\}$ converges and is:

- less than 1, then the series $\sum_{n=1}^{\infty} a_n$ converges;
- greater than 1, then the series $\sum_{n=1}^{\infty} a_n$ diverges;
- equals 1, then you can't tell what the hecks is going on.

### The integral test
If $f$ is a decreasing positive function such that $f(n) = a_n$ for all positive integers $n$, then:

$$
\int_{1}^{\infty} f(x) dx
$$

and

$$
\sum_{n = 1}^{\infty} a_n
$$

either both converge or both diverge.

### The alternating series test

The `alternating series test` says that if a series $\sum_{n = 1}^{\infty}$, and the absolute values of its terms are decreasing to $0$, then the series converges.

## How to Solve Series Problems

1. `Is the series geometric?` If your series only involves exponentials like $2^n$ or $e^{3n}$, it might be a geometric series, or it might be the sum of one or more geometric series.
2. `Do the terms got to 0?` If the series isn't geometric, try **n-th term test**.
3. `Are there negative terms in the series?` If so, you may have to use the **absolute convergence test** or the **alternating series test**.
4. `Are factorials involved?` If so, use **ratio test**
5. `Are there tricky exponentials with n in the base and the exponent?` If so, try **root test**.
6. `Do the terms have a factor ef exactly $1/n$ as well as logarithms?`, In this case, try **integral test**.
7. `Do none of the above tests seem to work?` You may have to use the **comparison test** or the **limit comparison test**.
