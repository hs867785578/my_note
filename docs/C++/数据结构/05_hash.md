# Hash Table

A `hash table` is a data structure that implements an associative array abstract data type, a structure that can map `keys` to `values`.

![hash](images/hash.png)

A `hash table` use a `hash function` to compute an index of `buckets` with `keys` to store desired `values`.

Ideally, the hash function will assign each key to a unique bucket, but most hash table designs employ an imperfect hash function, which might cause `hash collisions` where the hash function generate the same index for more than one key. The resolution of collision may contain:

- `seperate chaining`, if collision occers, we store both elements in the same bucket;

![chain](images/hash_chain.png)

- `linear probing`, when collision occers, we look for a new empty bucket to store the element.

## Problems

### Implementation

For the implementation of hash table, we can simply use a vector of linked list with the size of prime number. When addition, we just attach a new element at the end of linked list(bucket); and for deletion, we can swap the position of element to be deleted with the last element and then delete the last element.

- [Design Hashset](https://leetcode-cn.com/problems/design-hashset/)
- [Design Hashmap](https://leetcode-cn.com/problems/design-hashmap/)

### Duplication Detection(Hash Set)

We can use the hashset to check if an element have been visited while traversaling.

- [Contains Duplicate](https://leetcode-cn.com/problems/contains-duplicate/)
- [Single Number](https://leetcode-cn.com/problems/single-number/)

### Associating Keys with more Information(Hash Map)

We can use hashmap to create connections between keys and information about keys. For example, connecting the value and its index in an array.

- [Two Sum](https://leetcode-cn.com/problems/two-sum/)
- [Minimum Index Sum of Two Lists](https://leetcode-cn.com/problems/minimum-index-sum-of-two-lists/)

### Grouping Elements with Keys(Hash Map)

We can also group all elements with the same identity.

- [First Unique Charactor in a String](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)
- [Intersection of Two arrays II](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/)

### Designing Key to Use Hash

Sometimes we want to use the hash table to deal with problems, but the key of elements is not as simple as int, string, etc.

The general variants are:

1. Sorted string, if the order of charactors is not important;
2. Offset of each element;
3. Serialized string, if the element is a tree;
4. Row or Coloumn index, if you are dealing with matrix.

Problems:

- [Group Anagrams](https://leetcode-cn.com/problems/group-anagrams/)
- [Group Shifted Strings](https://leetcode-cn.com/problems/group-shifted-strings/)
- [Valid Sudoku](https://leetcode-cn.com/problems/valid-sudoku/)
- [Find Duplicate Subtrees](https://leetcode-cn.com/problems/find-duplicate-subtrees/)
