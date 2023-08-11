# Array

In computer science, an `array data structure`, or simply an `array`, is a data structure consisting of a cllection of elements(values or variables), each identified by at least one `array index` or `key`. An array is stored such that the position of each element can be computed from its index tuple by a mathematical formula. The simplest type of data structure is linear array, also called one-dimensional array. We can also use an array to store another array, which is called the 2-dimensional array.

Arrays are among the oldest and most important data structures, and are used by almost every program. They are also used to implement many other data structures, such as lists and strings. They effectively exploit the addressing logic of computers. In most modern computers and many external storage device, the memory is one-dimensional array of words, whose indices are their addresses. Processors, especially vector processors, are often optimized for array operations.

## Operations

The operations ant their time complexities of array are:

- Read an element by its index, $O(1)$;
- Look up an element, $O(N)$;
- Insert an element, $O(1)$ for back, $O(N)$ for random;
- Delete an element, $O(1)$ for back, $O(N)$ for random.

## Problems

Generally speaking, the problems about array are simple, but it may need more complex methods like:

- doubly pointer;
- dynamic programming;
- greedy;
- dfs;
- bfs;
