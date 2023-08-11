# Trie

In computer science, a `trie`, also called `digital tree` or `prefix tree`, is a type of `k-ary search tree`, a tree data structure used for locating specific keys from within as set. These keys are most often strings, with links between nodes defined not by the entire key, but by individual characters. 
Unlike a binary search tree, nodes in the trie do not store their associated key, Instead, a node's position in the trie defines the key with which it is associated.

![trie](images/trie.png)

## Implementation

An implementation of trie can be:

```C++
class Trie {
  struct Node {
    bool is_word{false};
    vector<Node*> child{};
    Node() : child(26, nullptr) {};
  };
  Node* root{new Node{}};
 public:
  void insert(const string& word) {
    auto node = root;
    for (auto c : word) {
      if (!node->child[c - 'a']) node->child[c - 'a'] = new Node{};
      node = node->child[c - 'a'];
    }
    node->is_word = true;
  }

  bool search(const string& word) {
    auto node = root;
    for (auto c : word) {
      if (!node->child[c - 'a']) return false;
      node = node->child[c - 'a'];
    }
    return node->is_word;
  }

  bool startsWith(const string& prefix) {
    auto node = root;
    for (auto c : prefix) {
      if (!node->child[c - 'a']) return false;
      node = node->child[c - 'a'];
    }
    return true;
  }
};
```

- [Implement Trie Prefix Tree](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)
- [Map Sum Pairs](https://leetcode-cn.com/problems/map-sum-pairs/)
- [Replace Words](https://leetcode-cn.com/problems/replace-words/)
- [Design Search Autocomplete System](https://leetcode-cn.com/problems/design-search-autocomplete-system/)
- [Design Add and Search Words Data Structure](https://leetcode-cn.com/problems/design-add-and-search-words-data-structure/)
- [Maximum Xor of Two Numbers in an Array](https://leetcode-cn.com/problems/maximum-xor-of-two-numbers-in-an-array/)
- [Word Search II](https://leetcode-cn.com/problems/word-search-ii/)
- [Word Squares](https://leetcode-cn.com/problems/word-squares/)
- [Palindrome Pairs](https://leetcode-cn.com/problems/palindrome-pairs/)
