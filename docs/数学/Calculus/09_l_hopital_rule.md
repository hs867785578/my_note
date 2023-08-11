# L'Hopital's Rule and Limits

We've used limits to find derivatives. Now we'll turn things upside-down and use derivatives to find limits, by way of a nive technique called `L'Hopital's Rule`.

Most of the limits we've looked at are naturally in one of the following forms:

- $\lim_{x \to a}\frac{f(x)}{g(x)}$
- $\lim_{x \to a}(f(x) - g(x))$
- $\lim_{x \to a}f(x)g(x)$
- $\lim_{x \to a}f(x)^{g(x)}$

Sometimes you can just substitute $x = a$ and evaluate the limit directly, effectively using the continuity of $f$ and $g$. This method doesn't always work, for example:

$$
\lim_{x \to 3} \frac{x^2 - 9}{x - 3}
$$

replacing $x$ by 3 gives the indeterminate form $\frac{0}{0}$.

It turns out that the first type, involving the ratio $\frac{f(x)}{g(x)}$, is the most suitable for applying the rule, so we'll call it `Type A`. The next two types, involving $f(x) - g(x)$ and $f(x)g(x)$, both reduce directly to `Type A`, so we'll call them `Type B1` and `Type B2` respectively. Finally, we'll say that limits involving exponentials like $f(x)^{g(x)}$ are `Type C`, since you can solve them by reducing them to `Type B2` and then back to `Type A`.

## Type A($0/0$ or $\pm \infty/ \pm \infty$)

Consider limits of the form:

$$
\lim_{x \to a} \frac{f(x)}{g(x)}
$$

where $f$ and $g$ are nice differentiable functions. If $g(a) \ne 0$, everything's great, you just substitude $x = a$ to see the limit is $\frac{f(a)}{g(a)}$. 

The only other possibility is that $f(a) = 0$ and $g(a) = 0$. That is, the fraction $\frac{f(a)}{g(a)}$ is the indeterminate form $\frac{0}{0}$. 
Let's return to the definition of limits:

$$
f'(x) = lim_{h \to 0} \frac{f(x + h) - f(x)}{h}
$$

Since $f$ and $g$ are differentiable, we can find the linearization of both of them at $x = a$. In fact, as we saw in the previous chapter, if $x$ is close to $a$, then:

$$
f(x) \cong f(a) + f'(a)(x - a)
$$

and 

$$
g(x) \cong g(a) + g'(a)(x - a)
$$

Now we assume that $f(a)$ and $g(a)$ are both $0$. This means:

$$
f(x) \cong f'(a)(x - a)
$$

and 

$$
g(x) \cong g'(a)(x - a)
$$

If you divide the first equation by the second one, then assuming that $x \ne a$, we get:

$$
\frac{f(x)}{g(x)} \cong \frac{f'(a)(x - a)}{g'(a)(x - a)} = \frac{f'(a)}{g'(a)}
$$

The closer $x$ is to $a$, the better the approximation. This leads to one version of l'Hopital's Rule:

> If $f(a) = g(a) = 0$, then

$$
\lim_{x \to a}\frac{f(x)}{g(x)} = \lim_{x \to a}\frac{f'(x)}{g'(x)}
$$

## Type B1($\infty - \infty$)
Here is a limit form:

$$
\lim_{x \to 0}(\frac{1}{sin(x)} - \frac{1}{x})
$$

As $x \to 0^+$, both $1/sin(x)$ and $1/x$ go to $\infty$. As $x \to 0^-$, both quantities go to $-\infty$. We can reduce this to `Type A`, just take a common denominator:

$$
\lim_{x \to 0}(\frac{1}{sin(x)} - \frac{1}{x}) = \lim_{x \to 0} \frac{x - sin(x)}{xsin(x)}
$$

Now you can put $x = 0$ and see $0/0$ case. So we can apply l'Hopital's Rule:

$$
\lim_{x \to 0} (\frac{1}{sin(x)} - \frac{1}{x}) = \lim_{x \to 0} \frac{x - sin(x)}{xsin(x)} = \lim_{x \to 0}\frac{1 - cos(x)}{sin(x) + xcos(x)} = \lim_{x \to 0} \frac{sin(x)}{cos(x) + cos(x) - xsin(x)} = 0
$$

## Type B2 ($0 \times \pm \infty$)

Here's the example:

$$
\lim_{x \to 0^+} x\ln(x)
$$

The idea is to move $x$ into a new denominator by putting it there as $\frac{1}{x}$:

$$
\lim_{x \to 0^+} x\ln(x) = \lim_{x \to 0^+} \frac{\ln(x)}{1/x}
$$

Now the form is $-\infty / \infty$, so we can use the l'Hopital's Rule:

$$
\lim_{x \to 0^+}x\ln(x) = \lim_{x \to 0^+}\frac{\ln(x)}{1/x} = \lim_{x \to 0^+}\frac{1/x}{-1/x^2} = 0
$$

## Type C($1^{\pm \infty}, 0^0, \infty^0$)

Finally, the trickiest type involves limits like:

$$
\lim_{x \to 0^+} x^{sin(x)}
$$

where both the base and exponent involve the dummy variable. The idea is to take the logarithm of the quatity $x^{sin(x)}$ first, and work out its limit as $x \to 0^+$:

$$
\lim_{x \to 0^+}ln(x^{sin(x)}) = \lim_{x \to 0^+}{sin(x)}\ln(x) = \lim_{x \to 0^+}ln(\frac{\ln(x)}{csc(x)}) = \lim_{x \to 0^+}ln(\frac{1/x}{-csc(x)cot(x)}) = ln(-1 \times 0) = ln(0) = 1
$$
