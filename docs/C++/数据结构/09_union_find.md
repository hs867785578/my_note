# Union-Find

A `union-find`, is a data structure that stores a collection of disjoint(non-overlapping) setting. Equivalently, it stores a partition of a set into disjoint subsets. It provides operations for adding new sets, merging sets and finding a representative member of a set.

```C++
class UnionFind {
 public:
  Union(int n) : size(n - 1, 1), cnt{n} {
    for (int i = 0; i < n; ++i) parent.push_back(i);
  }

 public:
  int Find(int x) {
    if (parent[x] != x) parent[x] = find(parent[x]);
    return parent[x];
  }

  bool Union(int x, int y) {
    int xx = find(x);
    int yy = find(y);
    if (xx == yy) return false;
    if (size[xx] > size[yy]) swap(xx, yy);
    parent[xx] = yy;
    size[yy] += size[xx];
    cnt--;
    return true;
  }

  int Count() { return cnt; }

  bool Connect(int x, int y) { return find(x) == find(y); }

 private:
  vector<int> parent{};
  vector<int> size{};
  int cnt{0};
};
```

Problems:

- [satisfiability-of-equality-equations](https://leetcode-cn.com/problems/satisfiability-of-equality-equations/)
- [number-of-provinces](https://leetcode-cn.com/problems/number-of-provinces/)
- [redundant-connection](https://leetcode-cn.com/problems/redundant-connection/)
- [number-of-operations-to-make-network-connected](https://leetcode-cn.com/problems/number-of-operations-to-make-network-connected/)
- [number-of-provinces](https://leetcode-cn.com/problems/number-of-provinces/)
- [evaluate-division](https://leetcode-cn.com/problems/evaluate-division/)
- [regions-cut-by-slashes](https://leetcode-cn.com/problems/regions-cut-by-slashes/)
- [swim-in-rising-water](https://leetcode-cn.com/problems/swim-in-rising-water/)
- [smallest-string-with-swaps](https://leetcode-cn.com/problems/smallest-string-with-swaps/)
- [most-stones-removed-with-same-row-or-column](https://leetcode-cn.com/problems/most-stones-removed-with-same-row-or-column/)
- [bricks-falling-when-hit](https://leetcode-cn.com/problems/bricks-falling-when-hit/)

