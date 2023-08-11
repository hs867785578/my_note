# Segment Tree

A `segment tree` is a data structure that allows answering range querying over an array effectively, while still being flexible enough to allow modifying the array. This includes finding the sum of consecutive array elements `a[l : r]`, or finding the minimum element in a such a range in $O(log n)$ time. Between answering such queries, the Segment Tree allows modifying the array by replacing one element, or even changing the elements of a whole subsegment (e.g. assigning all elements `a[l : r]` to any value, or adding a value to all element in the sub segment).

The root node of the segment tree is `1`, and for an array `a = {10, 11, 12, 13, 14}`, the segment tree will be like:

![segment tree](images/segtree.svg)

```C++
/// Segment tree for range sum
class SegmentTree {
 public:
  /// Reset data and change list to the size of 4n, where n is the size of array
  SegmentTree(const vector<int>& a) : d(4 * a.size(), 0), b(4 * a.size(), 0) {
    build(0, a.size() - 1, 1, a);
  }

  /// Recursively build the tree
  void build(int s, int e, int p, const vector<int>& a) {
    if (s == e) { d[p] = a[s]; return; }

    int m = s + (e - s) / 2;  // mid of the range
    build(s, m, p * 2, a)t
    build(m + 1, e, p * 2 + 1, a);
    d[p] = d[p * 2] + d[p * 2 + 1];
  }

  /// Pushdown the bias
  void pushdown(int s, int e, int m, int p) {
    if (!b[p]) return;
    // update current node's left and right child's data and bias
    // range [l, r] add a bias
    d[p * 2] += (m - s + 1) * b[p], d[p * 2 + 1] += (e - m) * b[p];
    b[p * 2] += b[p], b[p * 2 + 1] += b[p];
    b[p] = 0;  // clear current node's bias
  }

  /// Update the range [l, r] to c in the tree d[p], which is the range [s, e]
  void update(int l, int r, int c, int s, int e, int p) {
    if (l <= s && e <= r) {  // l <= s <= e <= r
      d[p] += (e - s + 1) * c;
      b[p] = c;
      return;
    }

    // range half to update
    int m = s + (e - s) / 2;
    pushdown(s, e, m, p);

    if (l <= m) update(l, r, c, s, m, p * 2);
    if (r > m) update(l, r, c, m + 1, e, p * 2 + 1);
    d[p] = d[p * 2] + d[p * 2 + 1];
  }

  /// Query sum of range [l, r] in the tree d[p], which is the range [s, e]
  int query(int l, int r, int s, int e, int p) {
    if (l <= s && e <= r) return d[p];

    int m = s + (e - s) / 2, sum = 0;
    pushdown(s, e, m, p);

    if (l <= m) sum += query(l, r, s, m, p * 2);
    if (r > m) sum += query(l, r, m + 1, e, p * 2 + 1);
    return sum;
  }

 private:
  vector<int> d{};  // tree node data array
  vector<int> b{};  // tree node bias array
};

/// Usage:
Vector<int> arr{11, 12, 13, 14, 15};
SegmentTree st{arr};
st.query(0, 2, 0, 4, 1);  // get sum of arr in range [0, 2], node 1 has range [0, 4]
st.update(0, 3, 5, 0, 4, 1);  // add(update) 5 to array in range[0, 2]
```

- [range-sum-query-mutable](https://leetcode-cn.com/problems/range-sum-query-mutable/)
