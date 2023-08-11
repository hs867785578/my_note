# Searching

## Breadth-first Search(BFS)

BFS is an algorithm for traversing or searching tree or graph data structures. It starts at the tree root(or some arbitrary node of a graph, sometimes referred as a `search key`), and explores all of the neighbor nodes at the present depth prior to moving on to the nodes at the next depth level.

![bfs](images/graph/bfs.gif)

```C++
void bfs(Graph& graph, int start) {
  deque<int> d{start};
  unordered_set<int> visited{start};
  while (!d.empty()) {
    int node = d.front();
    d.pop_front();
    for (auto next : graph[node]) {
      if (visited.count(next)) continue;
      visited.insert(next);
      d.push_back(next);
    }
  }
}
```

### 0-1 BFS

`0-1 BFS` is to solve the graph shortest path(SP) problem that the weight of `edge` can only be `0` or `1`.

We can solve this problem with Dijkstra or A star, but the time complexity may reach $O(E + VlogV)$, where `E` is the number of edges and `V` is the number of vertices.

`0-1 BFS` can reduce the time complexity ot $O(E + V)$, since it travals all the vertices and edges once.

Similar to Dijikstra, we only put a vertex in the queue if it has been relaxed by a previous vertex (distance is reduced by travelling on this edge) and we also keep the queue sorted by distance from source at every point of time.

Now, when we are at `u`, we know one thing for sure:
> travelling an edge `(u, v)` would make sure that `v` is either in the same level as `u` or at the next successive level.

This is because the edge weights are 0 and 1. An edge weight of 0 would mean that they lie on the same level, whereas an edge weight of 1 means they lie on level below. We also know that during BFS our queue holds vertices of two successive levels at max. So, when we are at vertex `u`, our queue contains elements of level $L[u]$ or $L[u] + 1$. And we also know that for an edge `(u, v)`, $L[v]$ is either $L[u]$ or $L[u] + 1$. Thus, if the vertex `v` is relaxed and has the same level, we can push it to the front of our queue and if it has the very next level, we can push it to the end of the queue. This helps us keep the queue sorted by level for the BFS to work properly.

But, using a normal queue data structure, we cannot insert and keep it sorted in $O(1)$. Using priority queue cost us $O(logN)$ to keep it sorted. The problem with the normal queue is the absence of methods which helps us to perform all of these functions:

1. Remove Top Element (To get vertex for BFS)
2. Insert At the beginning (To push a vertex with same level)
3. Insert At the end (To push a vertex on next level)

Fortunately, all of these operations are supported by a double ended queue:

```C++
int zero_one_bfs(Graph g, int src, int tar) {
  vector dis(g.size(), INT_MAX);
  dis[src] = 0;

  deque<int> dq{};
  dq.push_back(src);

  while(!dq.empty()) {
    auto node = dq.front(); dq.pop_front();

    for (auto next : g[node]) {
      int c = cost(node, next);
      if (dis[node] + c < dis[next]) {
        dis[next] = dis[node] + c;
        if (c == 1) {
          dq.push_back(next);
        } else {
          dq.push_front(next);
        }
      }
    }
  }

  return dis[tar];
}
```

Problems:

- [minimum-cost-to-make-at-least-one-valid-path-in-a-grid](https://leetcode.cn/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid/)

Reference:

- [codeforces 0-1 BFS](https://codeforces.com/blog/entry/22276?f0a28=1)

## Depth-first Search(DFS)

DFS is an algorithm for traversing or searching tree or graph data structure. The algorithm starts at the root node(selecting some arbitrary node as the root node in the case of a graph) and explores as far as possible along each branch before backtracking.

![dfs](images/graph/dfs.gif)

```C++
void dfs(Graph& graph, unordered_set<int>& visited, int root) {
  if (visited.count(root)) return;
  for (auto next : graph[root]) {
    dfs(graph, visited, next);
  }
}
```
