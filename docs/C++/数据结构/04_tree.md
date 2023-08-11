# Tree

A `tree` is a widely used abstract data type that simulate a hierarchical tree structure, with a root value and subtrees of children with a paarent node, represented as a set of linked nodes. Some nodes are:

![tree](images/tree.png)

- `node`: a structure wich may contain a value or condition, or represent a separate data structure.
- `child node`: each node in a tree has zero or more child node, which are below it in the tree.
- `parent node`: a node has a child is called the child's parent node(or `superior`).
- `acestor node`: a node's parent's parent, a node has at most one parent but possibly many ancestor nodes.
- `sibling nodes`: child nodes with the same paarent are  sibling nodes.
- `root node`: the tomost node in a tree.
- `branch nodes`: any node of a tree that has child nodes.
- `leaf nodes`: any node that does not have child nodes.
- `height` of a node: the length of the longest downward path to a leaf from that node.
- `depth` of a node: the length of the path to root.

The general solutions for tree often involves with recursion, a function which calls itself. We can use the following steps to define the recursive function:

1. Consider the return value;
2. Consider the parameter list;
3. Consider the terminal condition;
4. Consider the level logic.

## Binary Tree

`Binary Tree` is a kind of `tree` that each node has at most two children, which are refered to as the `left child` and `right child`.

![tree](images/bt.png)

### Traversal

There are mainly two methods to traversal a binary tree:

1. Depth First Search(BFS), and according to the order we visited the parent node, BFS can be divided into:
    1. `pre-order traversal`: first visiting parent node, and then left and right child;
    2. `in-order traversal`: first visiting left child, and then parent node, and then right child;
    3. `post-order traversal`: first visiting left and right child, and then parent node.
2. Bread First Search(BFS), which visits the tree layer by layer.

The general code for DFS is:

```C++
void dfs(TreeNode* root) {
  if (!root) return;

  // visit root->val here, pre-order
  dfs(root->left);
  // visit root->val here, in-order
  dfs(root->right);
  // visit root->val here, post-order
}
```

- [Binary Tree Preorder Traversal](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)
- [Binary Tree Inorder Traversal](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
- [Binary Tree Postorder Traversal](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

And for BFS:

```C++
void bfs(TreeNode* root) {
  if (!root) return;

  deque<TreeNode*> q{};
  q.push_back(root);

  while (!q.empty()) {
    auto size = q.size();
    while (size--) {  // visit each layer
      auto node = q.front();
      q.pop_front();

      if (node->left) q.push_back(node->left);
      if (node->right) q.push_back(node->right);
    }
  }
}
```

- [Binary Tree Level Order Traversal](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

There are other types of traversal, but all can be solved with above methods.

### Priorities

Sometimes we want to get the priority of a tree, like:

- Symmetric/Same
- Maximum/Minimum depth
- Balance
- Number of nodes
- Sum of the path from root to leaf node
- Common ancestor

Problems:

- [Symmetric Tree](https://leetcode-cn.com/problems/symmetric-tree/)
- [Maximum Depth of Binary Tree](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)
- [Minimum Depth of Binary Tree](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)
- [Count Complete Tree Nodes](https://leetcode-cn.com/problems/count-complete-tree-nodes/)
- [Balanced Binary Tree](https://leetcode-cn.com/problems/balanced-binary-tree/)
- [Binary Tree Paths](https://leetcode-cn.com/problems/binary-tree-paths/)
- [Lowest Common Ancestor of a Binary Tree](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

### Modify and Construct

- [Invert Binary Tree](https://leetcode-cn.com/problems/invert-binary-tree/)
- [Constract Binary Tree from Inorder and Postorder Traversal](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
- [Serialize and Deserialize Binary Tree](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

### Binary Search Tree

Binary Search Tree(BST) is a binary tree has the priority:

> Each branch node stores a key greater than all the keys in left subtree and less than those in its right substree.

The order of nodes in a BST means that each comparison skips about half of the remaining tree, so the whole lookup takes $log(N)$ time.

For BST problems, you can convert a BST to a sorted array by inorder traversal. Or, you can also use an extral pointer pointing to node visited last time in inorder traversal to get the left node in an sorted array.

#### Priority

##### Search

- [Search in a Binary Search Tree](https://leetcode-cn.com/problems/search-in-a-binary-search-tree/)

##### Check

- [Validate Binary Search Tree](https://leetcode-cn.com/problems/validate-binary-search-tree/)

##### Application

- [Minimum Absolute difference in BST](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/)
- [Find Mode in Binary Search Tree](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/)

#### Construct and Modify

##### Construct

- [Convert Sorted Array to Binary Search Tree](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

##### Insert

- [Insert into a Binary Search Tree](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)

##### Delete

- [Delete Node in a BST](https://leetcode-cn.com/problems/delete-node-in-a-bst/)

##### Trim

- [Trim a Binary Search Tree](https://leetcode-cn.com/problems/trim-a-binary-search-tree/)
