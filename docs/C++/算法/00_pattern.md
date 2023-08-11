# Algorithm Pattern

## Basic Algorithm

### Sort

#### Quick Sort
```C++
void quick_sort(int a[], int l , int r) {
  if (l >= r) return;

  int i = l - 1, j = r + 1, p = a[l + r >> 1];
  while (i < j) {
    while (a[++i] < p);
    while (a[--j] > p);
    if (i < j) swap(a[i], a[j]);
  }
  quick_sort(a, l, j);
  quick_sort(a, j + 1, r);
}
```

- [problem](https://www.acwing.com/problem/content/787/)

#### Merge Sort

```C++
int tmp[N];
void merge_sort(int a[], int l, int r) {
  if (l >= r) return;

  int mid = l + r >> 1;
  merge_sort(a, l, mid);
  merge_sort(a, mid + 1, r);

  int i = l, j = mid + 1, k = l;
  while (i <= mid && j <= r) tmp[k++] = a[i] < a[j] ? a[i++] : a[j++];
  while (i <= mid) tmp[k++] = a[i++];
  while (j <= r) tmp[k++] = a[j++];

  for (int k = l; k <= r; ++k) a[k] = tmp[k];
}
```

- [problem](https://www.acwing.com/problem/content/789/)


### Serach

#### Binary Search

```C++
//           Left                             Right
// |---------------------------| |------------------------------|
//                   right end | | left end

// Left end
// Devide [l, r] to [l, mid] + [mid + 1, r], l locates at mid + 1
int bs(int a[], int l, int r) {
  while (l < r) {
    int mid = l + r >> 1;
    if (resultl_is_in(mid)) {
      r = mid;
    } else {
      l = mid + 1;
    }
  }
  return l;
}

// Right end
// Devide [l, r] to [l, mid - 1] + [mid, r], l locates at mid
int bs(int a[], int l, int r) {
  while (l < r) {
    // avoid the case [l, r] = [0, 1]
    int mid = l + r + 1 >> 1;
    if (is_result_in(mid)) {
      l = mid;
    } else {
      r = mid - 1;
    }
  }
  return l;
}

/// Float
constexpr double eps = 1e-6;
double bs(double l, double r) {
  while (r - l > eps) {
    double mid = (l + r) / 2;
    if (Valid(mid)) {
      r = mid;
    } else {
      l = mid;
    }
  }
  return l;
}
```

- [problem](https://www.acwing.com/problem/content/791/)
- [problem](https://www.acwing.com/problem/content/792/)

### Expression

#### Addition

```C++
/// digits of A, B and C are in reverse order
vector<int> add(vector<int>& A, vector<int>& B) {
  if (A.size() < B.size()) return add(B, A);

  vector<int> C{};
  int t{0};
  for (int i = 0; i < A.size(); ++i) {
    t += A[i];
    if (i < B.size()) t += B[i];
    C.push_back(t % 10);
    t /= 10;
  }
  if (t) C.push_back(t);

  return C;
}
```

- [acwing](https://www.acwing.com/problem/content/793/)

#### Subtraction

```C++
/// digits of A, B and C are in reverse order
/// and A is bigger than B
vector<int> add(vector<int>& A, vector<int>& B) {
  vector<int> C{};
  for (int i = 0, t = 0; i < A.size(); ++i) {
    t = A[i] - t;
    if (i < B.size()) t -=B[i];
    C.push_back((t + 10) % 10);
    t = t < 0 ? 1 : 0;
  }
  while (C.size() > 1 && C.back() == 0) C.pop_back();
  return C;
}
```

- [problem](https://www.acwing.com/problem/content/794/)

#### Multiplication

```C++
/// digits of A and C are in reverse order
vector<int> mul(vector<int>& A, int b) {
  vector<int> C{};
  for (int i = 0, t = 0; i < A.size() || t; ++i) {
    if (i < A.size()) t += A[i] * b;
    C.push_back(t % 10);
    t /= 10;
  }
  while (C.size() > 1 && C.back() == 0) c.pop_back();
  return C;
}
```

- [problem](https://www.acwing.com/problem/content/795/)

#### Division

```C++
/// digits of A and C are in reverse order
pair<vector<int>, int> div(vector<int>& A, int b) {
  vector<int> C{};
  int r{0};
  for (int i = A.size() - 1; i >= 0; --i) {
    r = r * 10 + A[i];
    C.push_back(r / b);
    r %= b;
  }
  reverse(C.begin(), C.end());
  while (C.size() > 1 && C.back() == 0) C.pop_back();
  return {C, r};
}
```

- [problem](https://www.acwing.com/problem/content/796/)

### Prefix

#### Prefix Sum

```C++
for (int i = 1; i <= n; ++i) p[i] = p[i - 1] + a[i - 1];
```

- [problem](https://www.acwing.com/problem/content/797/)

#### Prefix Sum 2D

```C++
for (int i = 1; i <= n; ++i) for (int j = 1; j <= m; ++j)
    p[i][j] = a[i - 1][j - 1] + p[i][j - 1] + p[i - 1][j] - p[i - 1][j - 1];
```

- [problem](https://www.acwing.com/problem/content/798/)

#### Prefix Diff

```C++
void insert(int p[], int l, int r, int c) {
  p[l] += c;
  p[r + 1] -= c;
}

/// get each element
for (int i = 1; i <= n; ++i) p[i] += p[i - 1];
```

- [problem](https://www.acwing.com/problem/content/799/)

#### Prefix Diff 2D

```C++
void insert(int p[][N], int x1, int y1, int x2, int y2, int c) {
  p[x1][y1] += c;
  p[x2 + 1][y1] -= c;
  p[x1][y2 + 1] -= c;
  p[x2 + 1][y2 + 1] += c;
}

// Get each element
for (int i = 1; i <= n; ++i) for (int j = 1; j <= m; ++j)
  b[i][j] += b[i - 1][j] + b[i][j - 1] - b[i - 1][j - 1];
```

- [problem](https://www.acwing.com/problem/content/800/)

### Bit Operator

#### K bit

```C++
int kbit(int x, int k) { return x >> k & 1; }
```

- [problem](https://www.acwing.com/problem/content/803/)

#### Lowbit

```C++
int lowbit(int x) { return x & -x; }
```

### Two Pointers

```C++
/// 1 array
/// nnnnnnnnnnnnnnnnnnnnnn
///     ^          ^
///     | j        | i    
/// 2 array
/// nnnnnnnnnnnnnnnnnnnnnn
///     ^           
///     | i               
/// nnnnnnnnnnnnnnnnnnnnnn
///                ^
///                | j    
for (int i = 0, j = 0; i < n; ++i) {
  // add i here

  while (j < i && check(i, j)) ++j;  // remove j

  // record answer here
}
```

- [problem](https://www.acwing.com/problem/content/801/)
- [problem](https://www.acwing.com/problem/content/802/)

### Discrete

```C++
vector<int> a{};  // to be discrete
sort(a.begin(), a.end());
a.erase(unique(a.begin(), a.end()), a.end());

// get index after discrete
int find(int x) {
  int l = 0, r = a.size() - 1;
  while (l < r) {
    int mid = l + (r - l) / 2;
    if (a[mid] >= x) {
      r = mid;
    } else {
      l = mid + 1;
    }
  }
  return l;
}
```

- [problem](https://www.acwing.com/problem/content/804/)

### Merge Range

```C++
using PII = pair<int, int>;
void merge(vector<PII> &segs) {
  sort(segs.begin(), segs.end());

  vector<PII> ans{};
  for (auto [l, r] : segs) {
    if (ans.emtpy() || ans.back().second < l) {
      ans.push_back({l, r});
    } else {
      ans.back().second = max(ans.back().second, r);
    }
  }

  return ans;
}
```

- [problem](https://www.acwing.com/problem/content/805/)

## Data Structure

### Linked List

```C++
// head index , element, next element index, index to empty memory
int head, e[N], ne[N], idx;

void init() {
  head = -1;
  idx = 0;
}

// add a node with value of v to head
void to_head(int v) {
  e[idx] = v, ne[idx] = head, head = idx++;
}

// remove node after ith node
void remove(int i) {
  ne[i] = ne[ne[i]];
}

// insert a node with value of v after kth node
void insert(int k, int v) {
  e[idx] = v, ne[idx] = ne[k], ne[k] = idx++;
}

// traval the linked list
for (int i = head; i != -1; i = ne[i]) cout << e[i];
```

- [problem](https://www.acwing.com/problem/content/828/)

### Double Linked List

```C++

int e[N], l[N], r[N], idx;

// idx 0 for left node, idx 1 for right node
void init() {
  r[0] = 1, l[1] = 0;
  idx = 2;
}

// insert a node at i's right
void insert(int i, int v) {
  e[idx] = v;
  l[idx] = i, r[idx] = r[i];
  l[r[i]] = idx, r[i] = idx++;
}

// remove i node
void remove(int i) {
  r[l[i]] = r[i];
  l[r[i]] = l[i];
}

// travel
for (int i = 0; i != 1; i = r[i]) cout << e[i];
```

- [problem](https://www.acwing.com/problem/content/829/)

### Stack

```C++
int stk[N], idx = 0;

void push(int x) { stk[++idx] = x; }
void pop() { --idx; }
int top() { return stk[idx]; }
bool empty() { return idx == 0; }
```

- [problem](https://www.acwing.com/problem/content/description/830/)

### Queue

```C++
/// loop
int q[N], h = 0, t = 0;

void push(int x) { q[++t % N] = x; }
void pop() { h = ++h % N; }
void front() { return q[h]; }
bool empty() { return h == t; }
```

- [problem](https://www.acwing.com/problem/content/831/)

### Monotonous stack

```C++
int stk[N], t = 0;

for (auto n : a) {
  while (t && stk[t] >= n) --t;
  stk[++t] = n;
}
```

- [problem](https://www.acwing.com/problem/content/832/)

### Monotonous queue

```C++
/// h for head of queue, t for tail of queue, k for window size
/// nnnnnnnnnnnnnnnnnnnnnn
///     ^          ^
///     | h        | t    
int a[N], q[N];

for (int i = 0; i < n; ++i) {
  if (h <= t && i - q[h] + 1 > k) ++h;
  while (h <=t && a[q[t]] >= a[i]) --t;
  q[++t] = i;
  /// Check min value of the window
  printf("min value %d", a[q[h]]);
}

```

- [problem](https://www.acwing.com/problem/content/156/)

### KMP

```C++
char s[N], p[M], ne[N];

for (int i = 2, j = 0; i <= m; ++i) {
  while (j && p[j + 1] != p[i]) j = ne[j];
  if (p[j + 1] == p[i]) ++j;
  ne[i] = j;
}

for (int i = 1, j = 0; i <= n; ++i) {
  while (j && p[j + 1] != s[i]) j = ne[j];
  if (p[j + 1] == s[i]) ++j;
  if (j == m) {
    /// do something
    j = ne[j];
  }
}
```

- [problem](https://www.acwing.com/problem/content/833/)

### Trie

```C++
int son[N][26], cnt[N], idx;

void insert(char str[]) {
  int p = 0;
  for (int i = 0; str[i]; ++i) {
    int u = str[i] - 'a';
    if (!son[p][u]) son[p][u] = ++idx;
    p = son[p][u];
  }
  ++cnt[p];
}

/// return count of the str
int query(char str[]) {
  int p = 0;
  for (int i = 0; str[i]; ++i) {
    int u = str[i] - 'a';
    if (!son[p][u]) return 0;
    p = son[p][u];
  }

  return cnt[p];
}
```

- [problem](https://www.acwing.com/problem/content/837/)

### Union

```C++
/// origin Union, p for parent
int p[N]

// find parent
int find(int x) {
  if (p[x] != x) p[x] = find(p[x]);
  return p[x];
}

// init
for (int i = 1; i <= n; ++i) p[i] = i;

// merge
p[find(a)] = find(b);

/// Union with size
int p[N], size[N];

// find parent
int find(int x) {
  if (p[x] != x) p[x] = find(p[x]);
  return p[x];
}

// init
for (int i = 1; i <= n; ++i) p[i] = i;
for (int i = 1; i <= n; ++i) size[i] = 1;

// merge
size[find(a)] += size[find(b)];  // this must execute first
p[find(a)] = find(b);
```

- [problem](https://www.acwing.com/problem/content/838/)
- [problem](https://www.acwing.com/problem/content/839/)

### Heap

```C++
/// Minimum root heap
int h[N], size;

void down(int x) {
  int u = x;
  if (auto left = x * 2; left <= size && h[left] < h[u]) u = left; 
  if (auto right = x * 2 + 1; right <= size && h[right ] < h[u]) u = right; 
  if (u != x) {
    swap(h[u], h[x]);
    down(u);
  }
}

void up(int x) {
  while (x / 2 && h[x / 2] > h[x]) {
    swap(h[x / 2], h[x]);
    x >>= 1;
  }
}

/// O(N) build heap
for (int i = n / 2; i; --i) down(i);

/// Insert a number
h[++size] = x; up(size);

/// Get minimum
head[1];

/// Remove minimum(root)
swap(h[1], head[size--]); down(1);

/// Remove kth element
head[k] = head[size--]; down(k); up(k);

/// Modify kth element
head[k] = x; down(k); up(k);
```

- [problem](https://www.acwing.com/problem/content/840/)

### Hash table

```C++
/// Integar Hash

/// Seperate chaining
constexpr int N = 1e5 + 10;  // first prime number after length(1e5)
int h[N], e[N], ne[N], idx;  // h[n] for chain head ptr, e[n] for element, ne[n] for next ptr, idx for ptr

// init
memset(h, -1, sizeof(h));

void insert(int x) {
  int k = (x % N + N) % N;  // key, range in [0, N - 1]
  e[idx] = x;
  ne[idx] = h[k];
  h[k] = idx++;
}

bool find(int x) {
  int k = (x % N + N) % N;
  for (int i = h[k]; i != -1; i = ne[i]) if (e[i] == x) return true;
  return false;
}

/// Open addressing
constexpr int N = 2e5 + 3;  // 2 or 3 times of the number range, first prime
constexpr int null = 0x3f3f3f3f;  // a number bigger not in range to represent the nullptr
int h[N];

memset(h, 0x3f, sizeof(h));

int find(int x) {  // return the idx x locates or should be inserted
  int t = (x % N + N) % N;
  while (h[t] != null && h[t] != x) t %= ++t;
  return t;
}
h[find(x)] = x;  // insert
h[find(x)] == x;  // query 
```

- [problem](https://www.acwing.com/problem/content/842/)

```C++
/// string hash, to check if two substring on string are the same
using ULL = unsigned long long;  // number range [0, 2^64), overflow for mod
constexpr int N = 1e5 + 10, P = 131;  // P-base numeral system
ULL h[N], p[N];

// init
p[0] = 1;
for (int i = 1; i <= n; ++i) {
  h[i] = h[i - 1] * P + str[i];  // h for hash
  p[i] = p[i - 1] * P;  // p for P^i
}

ULL get(int l, int r) {
  return h[r] - h[l - 1] * p[r - l + 1];
}
```

- [problem](https://www.acwing.com/problem/content/843/)

## Graph

### Storage of Graph

```C++
/// adjecent matrix
g[a][b];  // for the edge a -> b

/// adjecent list
int h[N], e[N], ne[N], idx;

// add edge a -> b
void add(int a, int b) {
  e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

// init
idx = 0;
memset(h, -1, sizeof(h));
```

### DFS of graph

```C++
int dfs(int u) {
  st[u] = true;
  for (int i = h[u]; i != -1; i = ne[i]) {
    int j = e[i];
    if (!st[j]) dfs(j);
  }
}
```

- [problem](https://www.acwing.com/problem/content/848/)

### BFS of graph

```C++
queue<int> q{};
st[1] = true;
q.push(1);

while (!q.empty()) {
  int t = q.front();
  q.pop();

  for (int i = h[t]; i != -1; i = ne[i]) if (int j = e[i]; !st[j]) {
    st[j] = true;
    q.push(j);
  }
}
```

- [problem](https://www.acwing.com/problem/content/849/)

### Toposort

```C++
bool topsort() {
  int hh = 0, tt = -1;
  for (int i = 1; i <= n; ++i) if (!d[i]) q[++tt] = i;

  while (hh <= tt) {
    int t = q[hh++];
    for (int i = h[t]; i != -1; i = ne[i]) {
      if (int j = e[i]; --d[j] == 0) q[++tt] = j;
    }
  }

  return tt == n - 1;
}
```

- [problem](https://www.acwing.com/problem/content/850/)

### Shortest Path(SP)

![sp](images/cheatsheet/sp.png)

#### Original Dijkstra

```C++
/// for dense graph, node from 1 to n
int g[N][N], dist[N];
bool st[N];  // Store the node whose sp is determined

int dijkstra(int u) {
  memset(dist, 0x3f, sizeof(dist));
  dist[u] = 0;

  for (int i = 0; i < n - 1; ++i) {
    // find the node who's not in st but has minimum distance to u
    int t = -1;
    for (int j = 1; j <= n; ++j) if (!st[j] && (t == -1 || dist[j] < dist[t]))
      t = j;

    // use t to update all other nodes' distance
    for (int j = 1; j <= n; ++j) dis[j] = std::min(dist[j], dist[t] + g[t][j]);

    // put t to st
    st[t] = true;
  }

  return dist[n] == INF ? -1 : dist[n];
}
```

- [problem](https://www.acwing.com/problem/content/851/)

#### Heap-optimal Dijkstra

```C++
/// for sparse graph, node from 1 to n
using PII = piar<int, int>;
int h[N], e[M], w[M], ne[M], idx;  // e for element, c for cost, ne for next
memset(h, -1, sizeof(h));
void add(int x, int y, int z) {
  e[idx] = y, w[idx] = z, ne[idx] = h[x], h[x] = idx++;
}

int dist[N];
bool st[N];

int dijkstra(int u) {
  memset(dist, 0x3f, sizeof(dist));
  dist[u] = 0;

  priority_queue<PII, vector<int>, greater<PII>> pq{};
  pq.push({0, u});

  while (pq.size()) {
    auto [dis, node] = pq.top();
    pq.pop();

    if (st[node]) continue;
    st[node] = true;

    for (int i = h[node]; i != -1; i = ne[i]) {
      int next = e[i], cost = w[i];
      if (dist[next] > dist[node] + cost) {
        dist[next] = dist[node] + cost;
        pq.push({dist[next], next});
      }
    }
  }

  return dist[n] == INF ? -1 : dist[n];
}
```

- [problem](https://www.acwing.com/problem/content/852/)

#### Bellman-Ford

```C++
int dist[N], back[N];
struct Edge { int a, b, w; } es[M];

int bellman_ford(int u) {
  memset(dist, 0x3f, sizeof(dist));
  dist[u] = 0;

  // relax k times, all sps within k edges are found
  for (int i = 0; i < k; ++i) {
    memcpy(back, dist, sizeof(dist));
    for (int j = 0; j < m; ++j) {
      auto [a, b, w] = es[j];
      dist[b] = std::min(dist[b], back[a] + w);
    }
  }

  return dist[n];
}
```

- [problem](https://www.acwing.com/problem/content/855/)

#### spfa

```C++
/// Shortest Path
int h[N], w[N], e[N], ne[N], idx;
void add(int a, int b, int c) {
  w[idx] = c, e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}
int dist[N];
bool st[N];

int spfa() {
  memset(dist, 0x3f, sizeof(diat));
  dist[0] = 0;
  st[0] = true;

  queue<int> q{};
  q.push(0);

  while (q.size()) {
    auto t = q.front();
    q.pop();

    st[t] = false;

    for (int i = h[t]; i != -1; i = ne[i]) {
      int j = e[i];
      if (dist[j] > dist[t] + w[i]) {
        dist[j] = dist[t] + w[i];
        if (!st[j]) {
          q.push(j);
          st[j] = true;
        }
      }
    }
  }

  return dist[n] = 0x3f3f3f3f ? -1 : dist[n];
}
```

- [problem](https://www.acwing.com/problem/content/853/)

```C++
/// Check negtive cycle
int h[N], e[N], w[N], ne[N], idx;
void add(int a, int b, int c) {
  e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
}

int dist[N], cnt[N];
bool st[N];

bool spfa() {
  queue<int> q{};
  for (int i = 1; i <= n; ++i) {
    st[i] = true;
    q.push(i);
  }

  while (q.size()) {
    auto t = q.front();
    q.pop();

    st[t] = false;

    for (int i = h[i]; i != -1; i = ne[i]) {
      int j = e[i];
      if (dist[j] > dist[t] + w[i]) {
        dist[j] = dist[t] + w[i];
        cnt[j] += cnt[t] + 1;
        if (cnt[j] >= n) return false;

        if (!st[j]) {
          q.push(j);
          st[j] = true;
        }
      }
    }
  }

  return false;
}
```

- [problem](https://www.acwing.com/problem/content/854/)

#### Floyd

```C++
/// init
for (int i = 1; i <=n; ++i) {
  for (int j = 1; j <= n; ++j) {
    dist[i][j] = i == j ? 0 : INF;
  }
}

/// Floyd
for (int k = 1; k <= n; ++k) {
  for (int i = 1; i <= n; ++i) {
    for (int j = 1; j <= n; ++j) {
      dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
    }
  }
}
```

- [problem](https://www.acwing.com/problem/content/856/)

### Minimum Spanning Tree

`Prim` for dense graph, `Kruskal` for sparse graph.

#### Prim

```C++
int g[N][N], dist[N];
bool st[N];

int prim() {
  int ans = 0;
  memset(dist, 0x3f, sizeof(dist));
  for (int i = 0; i < n; ++i) {
    int t = -1;
    for (int j = 1; j <= n; ++j) if (!st[j] && (t == -1 || dist[j] < dist[t]))
      t = j;

    if (i && dist[t] == INF) return INF;
    if (i) ans += dist[t];

    st[t] = true;
    for (int j = 1; j <= n; ++j) dist[j] = min(dist[j], g[t][j])
  }

  return ans;
}
```

- [problem](https://www.acwing.com/problem/content/860/)

#### Kruskal

```C++
int p[N];
int find(int x) {
  if (p[x] != x) p[x] = find(p[x]);
  return p[x];
}

struct Edge {
  int a, b, w;
  bool operator<(const Edge& rhs) const { return w < rhs.w; }
} edges[M];

int kruskal() {
  sort(edges, edges + m);

  int ans = 0, cnt = 0;
  for (int i = 0; i < m; ++i) {
    auto [a, b, w] = edges[i];
    a = find(a), b = find(b);
    if (a != b) {
      p[a] = b;
      and += w;
      ++cnt;
    }
  }

  return cnt < n - 1 ? INF : ans;
}
```

- [problem](https://www.acwing.com/problem/content/861/)

### Bipartite Graph

#### Coloring (check bipartite graph)

```C++
int color[N];  // -1 for uncolored, 0 for white, 1 for black

bool dfs(int u, int c) {
  color[u] = c;
  for (int i = h[u]; i != -1; i = ne[i]) {
    int j = e[i];
    if (color[j] == -1 && !dfs(j, !c)) return false;
    if (color[j] == c) return false;
  }

  return true;
}

for (int i = 1; i <= n; ++i) if (!dfs(i, 0)) return false;
return true;
```

- [problem](https://www.acwing.com/problem/content/862/)

#### Maximum Bipartite Matching(MBM), Hungarian

```C++
int match[N];  // 0 for unmatched
bool st[N];

bool find(int x) {
  for (int i = h[x]; i != -1; i = ne[i]) {
    int j = e[i];
    if (!st[j]) {
      st[j] = true;
      if (match[j] == 0 || find(match[j])) {
        match[j] = x;
        return true;
      }
    }
  }

  return false;
}

int ans = 0;
for (int i = 1; i <= n1; ++i) {
  memset(st, false, sizeof(st));
  if (find(i)) ++ans;
}
```

- [problem](https://www.acwing.com/problem/content/863/)

## Dynamic Programing(DP)

### Knapsack Problem

- [01 package](https://www.acwing.com/problem/content/2/)
- [unbounded package](https://www.acwing.com/problem/content/3/)
- [bounded package](https://www.acwing.com/problem/content/4/)
- [bounded package(optimal)](https://www.acwing.com/problem/content/5/)
- [group package](https://www.acwing.com/problem/content/9/)

### Linear Dp

- [Triangle](https://www.acwing.com/problem/content/900/)
- [Longest Increasing Subsequence(LIS)](https://www.acwing.com/problem/content/897/)
- [Longest Increasing Subsequence(optimal)](https://www.acwing.com/problem/content/898/)
- [Longest Common Subsequence(LCS)](https://www.acwing.com/problem/content/899/)
- [Edit Distance](https://www.acwing.com/problem/content/898/)

### Range Dp

- [Merge Stone](https://www.acwing.com/problem/content/284/)

### Count Dp

### Digital Statistic Dp

### State Compress Dp

### Tree Dp

### Memory Search
