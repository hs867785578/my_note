# Algorithm Analysis

An `algorithm` is a clearly specified set of simple instructions to be followed to solve a problem. The important step of algorithm analysis is to determine how much in the way of resource, such as time or spaces, the algorithm requires.

## General Rules
Since we are giving the running time in terms of `Big-Oh`, there are lots of shortcuts that can be taken without affecting the final answer.

### For Loops
The running time of a `for` loop is at most the running time of the statements inside the `for` loop times the number of iterations.

### Nested Loops
The total running time of a statement inside a group of nested loops is the running time of the statement multiplied by the product of the sizes of all the loops.

### Consecutive Statements
Just add them all. The maximum is the one that counts.

### If/else
The running of an `if/else` statement is never more than the running time of the larger of the running time of two statements.

### Stratagy of Analyzing Recursion
If the recursion is really just a thinly veiled `for` loop, the analysis is usually trivial.

If the recursion is properly used, it is difficult to convert the recursion into a simple loop structure. In this case, the analysis will involve a recurrence relation that needs to be solved.

We firstly let $T(N)$ be the running time for current function, and then running time involved with recursion is represented, for example:
$$
T(N) = T(N - 1) + T(N - 2)
$$
And then we can solve the equation and analysis the approximate running time.

### Logarithms in the Running Time
An algorithm is $O(N)$ if it take constant time to cut the problem size by a fraction (which is usually $\frac{1}{2}$). Otherwise, if constant time is required to merely reduce the problem by a constant amount(such as 1), the algorithm is $O(N)$.

## General Input Size and Maximum Time Complexity

| Input Size(N) | Worst Accepted Algorithm | Algorithm Type |
| --- | --- | --- |
| $< 10$ | $O(N!)$ | Permutation |
| $< 15$ | $O(2^N)$ | Combination |
| $< 50$ | $O(N^4)$ | DP |
| $< 200$ | $O(N^3)$ | DP |
| $< 1000$ | $O(N^2)$ | DP |
| $< 10^6$ | $O(N)$ or $O(N\log(N))$ | DP, Greedy, Heap, Divide&Conquer |
| $< 10^8$ | $O(log(N))$ or $O(1)$ | Binary Search, Math |
