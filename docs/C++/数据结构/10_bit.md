# Fenwick Tree

![fenwick](images/fenwick.svg)

A `Fenwick tree` or `binary indexed tree` is a data structure that can efficiently update elements and calculate prefix sums in a table of numbers.

When compared with a flat array of numbers, the Fenwick tree achieves a much better balance between two operations:

- element udpate;
- prefix sum calculattion.

A flat array of `n` numbers can either store the elements or the prefix sums. Fenwick trees allow both operations above to be performed in $O(logn)$ time. This is achieved by representing the numbers as a tree, where the value of each node is the sum of the numbers in that subtree.

| Algorithm | Time Complexity |
| --------- | --------------- |
| Space | $O(n)$ |
| Search | $O(logn)$ |
| Insert | $O(logn)$ |
| Delete | $O(logn)$ |


A Fenwick tree is most easily understood by considering a one-based array. Each element whose index `i` is a power of 2 contains the sum of the first `i` elements. Elements whose indices are the sum of two (distinct) powers of 2 contains the sum of the elements since the preceding power of 2. In general, each element contains the sum of the values since its parent in the tree, and that parent is found by clearing the least significant bit in the index.

```C++
class FenwickTree {
 public:
  NumArray(vector<int>& nums) : tree(nums.size() + 1, 0), arr{nums} {
    for (int i = 0; i < arr.size(); ++i) add(i + 1, arr[i]);
  }

  void update(int x, int val) {
    add(x + 1, val - arr[x]);
    arr[x] = val;
  }

  int sumRange(int l, int r) {
    return query(r + 1) - query(l);
  }

 private:
  int query(int x) {
    int ans = 0;
    for (; x; x -= x & -x) ans += tree[x];
    return ans;
  }

  void add(int x, int v) {
    for (; x < tree.size(); x += x & -x) tree[x] += v;
  }

  vector<int> tree{};
  vector<int> arr{};
};
```

- [range-sum-query-mutable](https://leetcode-cn.com/problems/range-sum-query-mutable/)
