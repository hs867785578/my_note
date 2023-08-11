# Queue

In computer science, a queue is a collection of entities that are maintained in a sequence and can be modified by the addition of entities at one end of the sequence and the removal of entities from the other end of the sequence. By convention, the end of the sequence at which elements are added is called the back, tail, or rear of the queue, and the end at which elements are removed is called the head or front of the queue, anologously to the words used when people line up to wait for goods or services.

![queue](images/queue.png)

The operations of a queue make it a `first-in-first-out(FIFO)` data structure. Common implementations of queue are:

- circular buffers
- linked lists

## Problems

### Implementation

- [232. Implement Queue Using Stack](https://leetcode-cn.com/problems/implement-queue-using-stacks/)
- [346. Moving Average from Data Stream](https://leetcode-cn.com/problems/moving-average-from-data-stream/)
- [622. Design Circular Queue](https://leetcode-cn.com/problems/design-circular-queue/)

### Breadth First Search

A common use of BFS is to find the shortest path, in other words, the minimum steps from start state to the target state. If the problem is about states transforming and we can traverse all states step by step, we can use DFS to find the answer.

The general pattern for BFS is:

```C++
queue<Node> q{};
q.push(start_node);
while (!q.empty()) {
  auto node = q.front();
  q.pop();

  for (auto n : node.next_nodes()) {
    q.push_back(n);
  }
}
```

And if we want to count how many times (steps) the state transformation happens, we should use deque and add the counter:

```C++
deque<Node> q{};
q.push_back(start_node);
int step = 0;
while (!q.empty()) {
  auto size = q.size();
  while (size--) {
    auto node = q.front();
    q.pop_front();

    for (auto n : node.next_nodes()) {
      q.push_back(n);
    }
  }
  ++step;
}
```

And more often, we do not want to visit a state twice(because the state is viisted and there is no need to do it again), we can choose which node can enqueue:

```C++
deque<Node> q{};
q.push_back(start_node);
unordered_set<Node> visited{};
visited.insert(start_node);
int step = 0;
while (!q.empty()) {
  auto size = q.size();
  while (size--) {
    auto node = q.front();
    q.pop_front();

    for (auto n : node.next_nodes()) {
      if (visited.count(n)) continue;
      visited.insert(n);
      q.push_back(n);
    }
  }
  ++step;
}
```

- [Walls and Gates](https://leetcode-cn.com/problems/walls-and-gates/)
- [Number of Islands](https://leetcode-cn.com/problems/number-of-islands/)
- [Open the Lock](https://leetcode-cn.com/problems/open-the-lock/)
- [Sliding Window Maximum](https://leetcode-cn.com/problems/sliding-window-maximum/)

## Reference

- [LeetBook Queue and Stack](https://leetcode-cn.com/leetbook/detail/queue-stack/)
