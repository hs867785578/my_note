# K-d Tree

A k-d tree(short for k-dimension tree) is a space-partitioning data structure for organizing points in a k-dimensional space. k-d trees are a useful data structure for several applications, such as searches involving a multidimensional search key (e.g. range searches and nearest neighbor searches). k-d trees are a special case of binary space partitioning trees.

k-d trees are not suitable, however, for efficiently finding the nearest neighbor in high dimensional spaces. As a general rule, if the dimensionality is `k`, the number of point in the data is `N`, should be:

$$
N \gg 2^k
$$

Otherwise, when k-d trees are used with high-dimensional data, most of the points in the tree will be evaluated and the efficiency is no better than exhaustive search, and other methods such as approximate nearest-neighbor are used instead.

```C++
#include <algorithm>
#include <array>
#include <vector>
#include <iostream>
#include <cmath>
#include <functional>

/// @class the KdTree Implementation
template<typename T, size_t D>
class KdTree {
 public:
  using Point = std::array<T, D>;

 private:
  /// @struct Kdtree node defination
  struct Node {
    Point point{};
    Node* left{nullptr};
    Node* right{nullptr};
  };

  /// @struct Less compare operator for two points 
  struct NodeComp {
    size_t index{0};

    bool operator()(const Point& a, const Point& b) {
      return a[index] < b[index];
    }
  };

  /// Build the tree with a list of points recursively
  /// @return root of the tree
  Node* build_tree(
      std::vector<Point>& pts, size_t begin, size_t end, size_t dim) {
    if (begin >= end) return nullptr;

    size_t mid = begin + (end - begin) / 2;
    auto it = pts.begin();
    std::nth_element(it + begin, it + mid, it + end, NodeComp{.index = dim});
    dim = ++dim % D;

    return new Node{pts[mid], build_tree(pts, begin, mid, dim),
                              build_tree(pts, mid + 1, end, dim)};
  }

  /// Calculate the square distance of two points
  /// @return squre distance
  double sqr_dis(const Point& p1, const Point& p2) {
    double ans{0};
    for (size_t i = 0; i < D; ++i) ans += (p1[i] - p2[i]) * (p1[i] - p2[i]);
    return ans;
  }

  /// Get the nearest point and distance by the given point
  void nearest(const Node* root, const Point& tar, size_t dim, Point& best,
               double& min_dis) {
    if (!root) return;

    const double d = sqr_dis(root->point, tar);
    if (d < min_dis) {
      min_dis = d;
      best = root->point;
    }

    const double dx = root->point[dim] - tar[dim];
    dim = ++dim % D;

    nearest(dx > 0 ? root->left : root->right, tar, dim, best, min_dis);
    if (dx * dx > min_dis) return;
    nearest(dx > 0 ? root->right : root->left, tar, dim, best, min_dis);
  }

 public:
  /// Copy control
  KdTree(const KdTree&) = delete;
  KdTree& operator=(const KdTree&) = delete;
  KdTree(KdTree&&) = delete;
  KdTree& operator=(KdTree&&) = delete;

  ~KdTree() {
    std::function<void(Node*)> f = [&f](Node* root) {
      if (!root) return;
      f(root->left);
      f(root->right);
      delete root;
      root = nullptr;
      return;
    };

    f(root);
  }

  /// Constructor with a list of points
  KdTree(std::vector<Point> pts) {
    root = build_tree(pts, 0, pts.size(), 0);
  }

  /// Get the nearest point and distance with given point
  /// @return point and distance of the nearest
  std::pair<Point, double> Nearest(const Point& tar) {
    if (!root) return {};

    double min_dis{std::numeric_limits<double>::max()};
    Point ans{};
    nearest(root, tar, 0, ans, min_dis);

    return {ans, std::sqrt(min_dis)};
  }

 private:
  Node* root{nullptr};
};

/// Test code
int main() {
  std::vector<std::array<double, 2>> pts{
      {2, 3}, {5, 4}, {9, 6}, {4, 7}, {8, 1}, {7, 2}};
  KdTree<double, 2> kdt{pts};

  auto [pt, dis] = kdt.Nearest({9, 2});
  std::cout << "nearest: (" << pt[0] << ", " << pt[1] << ") in " << dis
            << std::endl;
}
```

## Reference
- [K-d Tree](https://rosettacode.org/wiki/K-d_tree)
