# Stack

Stacks and queues are dynamic sets in which the element removed from the set by `Delete` operation is prespecified.

In a `stack`, the element deleted from the set is the most recently inserted: the stack implements a `last-in, first-out` or `LIFO`, policy.

![stack](images/stack.png)

The `Insert` operation on a stack is often called `Push`, and the `Delete` operation, which does not take an element, is often called `Pop`. These names are allusions to physical stacks, such as the spring-loaded stacks of plates used in cafeteriass. The order in which plates are popped from the stack is the reverse of the order in which they were pushed onto the stack, since only the top plate is accessible.

## Problems

### Implementation

This kind of problems mainly contains use other data structures of:

- array
- linked list
- queue

to implement a `stack` class.

- [155. Min Stack](https://leetcode-cn.com/problems/min-stack/)
- [225. Implement Stack using Queues](https://leetcode-cn.com/problems/implement-stack-using-queues/)

### Syntax Parsing
This kind of problems are mainly about the few elements at the end of lists, for example:

- Paretheses matching, which delete the parentheses matched;
- Path finding, which delete the elements at the back if `..` occurs.

problems:

- [20. Valid Parentheses](https://leetcode-cn.com/problems/valid-parentheses/)
- [71. Simplify Path](https://leetcode-cn.com/problems/simplify-path/)

### Expression Evaluation 

The tytical problem of this is the calculator. We usually solve this with two stacks:

- `number` stack, which records all numbers to be calculated；
- `operator` stack, which stores operators(`(, ), +, -, *, /`) in increasing grade, `(, )` is 0, `+, -` is 1, `*, /` is 2.

The general frame is:
```C++
stack<string> ops;
stack<int> nums;
int i = 0;
while (i < str.size()) {
  if (is_left_parenth) {
    // push to ops
    ++i;
  } else if (is_right_parenth) {
    // calculate numbers and operators util ops.top() is left parenth
    // ops.pop()
    ++i
  } else if (is_number) {
    // get the whole number
    // push the number to number stack
    ++i
  } else if (is_operator) {
    // if curr_op <= ops.top()
    // calculate the operators
    // push curr_op to operator stack
    ++i
  }
}
```

- [150. Evaluate Reverse Polish Notation](https://leetcode-cn.com/problems/evaluate-reverse-polish-notation/)
- [224. Basic Calculator](https://leetcode-cn.com/problems/basic-calculator/)
- [227. Basic Calculator II](https://leetcode-cn.com/problems/basic-calculator-ii/)
- [772. Basic Calculator III](https://leetcode-cn.com/problems/basic-calculator-iii/)

### Monotonous Stack

Monotonous stack make sure that all elements from bottom to top are in increasing or decreasing order. To do so, use the loop:

```C++
stack<int> ss{};
for (auto n : nums) {
  while (!ss.empty() && n <= ss.top()) ss.pop();
  ss.push(n);
}
```

Sometimes we want to choose `n` number form a list, the relative order of the digits from the list should be preserved and make the new number with the length of `n` maximum.

This could also be created with monotonous stack:

```C++
int drop = arr.size() - n;
stack<int> ss{};
for (auto n : nums) {
  while (drop && !ss.empty() && ss.top() <= n) { ss.pop(); --drop; }
  ss.push(n);
}
while (ss.size() > n) ss.pop();
```

- [42. Trapping Rain Water](https://leetcode-cn.com/problems/trapping-rain-water/)
- [84. Largest Rectangle in Histogram](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)
- [321. Create Maximum Number](https://leetcode-cn.com/problems/create-maximum-number/)

