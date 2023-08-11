# Linked List

A `linked list` is a data structure in which the objects are arranged in a linear order. Unlike an array, however, in which the linear order is determined by the array indeces, the order in a linked list is determined by a pointer in each object. Linked lists provide a simple, flexible representation for dynamic sets, supporting (though not necessaryily efficiently) all the operations.

Each record of a linked list is often called an `node`.

The field of each node that contains the address of the next node is usually called the `next link` or `next pointer`. The remaining fields are known as the `data`.

## Types

Generally, there are two kinds of linked list:

- singly linked list
- Doubly linked list

The operation complexities are:

| Insert/Delete | Check/Modify |
| --- | --- |
| O(1) | O(n) |

### Singly Linked List

Singly linked lists contain nodes which have a data field as well as `next`, which points to the next node in line of nodes.

```C++
struct Node {
  int val{0};
  Node* next{nullptr};
};
```

### Doubly Linked List

In a doubly linked list, each node contains, besides the next-node link, as second link field pointing to the `previous` node in the sequence.

```C++
struct Node {
  int val{0};
  Node* prev{nullptr};
  Node* next{nullptr};
};
```

## Problems

The problems about linked list are generally easy, but it may take much attention to care about:

- check the if pointer of linked list is valid
- check if the node is correct while traveling the linked list
- sometimes it will simplify the code if we add a dump node at the head or tail of linked list.

### Implementation

Implementation involves operations of delete and add:

- for singly linked list

```C++
// Add after node
auto next = new Node{val, node->next};
node->next = next;

// Delete the node behind
node->next = node->next->next;
```

- for doubly linked list

```C++
// Add after node
auto next = new Node{val, node, node->next};
next->prev->next = next;
next->next->prev = next;

// Delete the node behind
node->next->next->prev = node;
node->next = node->next->next;
```

- [Design Linked List](https://leetcode-cn.com/problems/design-linked-list/)

### Reverse Linked List

This problem often works as the intermediate step in other more complex problems.

The main idea is to put the node to the head while traversing all the nodes.

```C++
auto node = head->next;
    head->next = nullptr;
    while (node) {
      auto tmp = node->next;
      node->next = head;
      head = node;
      node = tmp;
    }
```

- [Reverse Linked List](https://leetcode-cn.com/problems/reverse-linked-list/)

### Double Pointer

The typical usage of double pointers is to check if the linked list is circular and the entry of loop. In this skill we have two pointers:

- slow pointer, which goes one step once;
- faster pointer, which goes two steps once.

```C++
auto slow = head;
auto fast = head->next;
while (fast && fast->next) {
  slow = slow->next;
  fast = fast->next->next;
}
```

If two pointers meet at the same node, the linked list is circular.

In other situations, double pointers of slow and fast can divide the linked into two equal parts, because fast pointer goes twice of the slow, and when fast stops at the tail, the slow stops at the middle.

- [Linked List Cycle](https://leetcode-cn.com/problems/linked-list-cycle/)
- [Linked List Cycle II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)
- [Intersection of Two Linked List](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)
- [Remove Nth Node from End of List](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

### Other

- [Flateen a Multilevel Doubly Linked List](https://leetcode-cn.com/problems/flatten-a-multilevel-doubly-linked-list/)
- [Copy List with Random Pointer](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)
