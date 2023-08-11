In computer science, `A*`(pronounced *A-Star*) is a computer algorithm that is widely used in pathfinding and graph traversal, which is the process of finding a path between multiple points, called "nodes". It enjoys widespread use due to its performance and accuracy. Peter Hart, Nils Nilsson and Bertram Raphael of Stanford Research Institude(now SRI International) first published the algorithm in 1968. It can be seen as an extension of Edsger Dijkstra's 1959 algorithm. `A*` achieves better performance by using heuristics to guide its search.
## Problem
In real world, we often want to find a path from one location to another, it may be the shortest one or the most suitable one. Movement for a single object seems easy, pathfinding is complex, but pathfinder let you plan a path before you meet the obstacles, then you can move the short or suitable path. Path planning generally is slow, but gives better results.

## Assumption
To find the path, we can sue a `graph search` algorithm, which works when the map is represented as a graph.

### Input
Graph search algorithms take a `graph` as input. A graph is a set of locations("node") and the connections("edge") between them, like this:

![graph](images/a_star/graph.png)

Pathfinder doesn't see anything else, it only see the graph.

### Output
The path found by pathfinder is made of `graph nodes and edges`. It tells you to move from one location to another, but it can't tell you how. You have to decide whether a graph edge you got means walking in a straight line or running along a curved path.

## Early Solutions
There are lots of algorithms that run on graphs and find the path, the simplest is "Breadth First Search".

### Breadth First Search
**Breadth First Search** explores equally in all directions. The key idea for this algorithm is that we keep track of an expanding ring called `frontier`. On a grid, this process is somtimes called "flood fill", but the same technique works for non-grids.
![breadth](images/a_star/breadth_first.gif)

The main process is:

Loop:

- Pick and remove a `location` from the `frontier`;
- Expand it by looking at its `neighbors`, any neighbors we haven't visited yet we add to the `frontier`, and also to the `visited` set;
- if `frontier` is empty or reach the target node, break.

So far we assume that every step has the same `cost`, however, in some scenarios there are difference costs for different types of movements. For example, a diagonal movement on a grid costs more than axial movement. We'd like the pathfinder to take these costs into account. For this purpose, we use `Dijkstra's Algorithm`

### Dijkstra's Algorithm
Dijkstra's Algorithm tracks movement cost from the start location. The pathfinder may visit a location multiple times, with different costs. Instead of visit every node in `frontier`, we visit the lowest cost node at the beginning of the loop.
The main process is below:

- Create a priority queue `frontier` and put the init node into `frontier`.
- Loop:
    - choose the lowest cost node from `frontier`
    - if `frontier` is empty or current node is goal node, break;
    - get current node's neighbours, for each neighbour:
        - if the neighbour node has not been visited yet or its cost is lower than current node:
            - put the neighbour node into `frontier`
            - set the cost so far as the neighbour node's cost

![breadth_first](images/a_star/dijkstra.gif)

With Breadth First Search and Dijkstra's Algorithm, the frontier expands in all directions. This is a reasonable choice if you only need to find a location and don't care about the time. However, a common case is that we want to find the path not only correctly but also quickly. Here comes the `Greedy Best First Search`.

### Greedy Best First Search
Greedy Best First Search use the estimated distance to the goal for priority queue ordering. The location closet to the goal will be explored first.
Its process can be described as:

- Create a priority queue `frontier` and put the init node into `frontier`.
- Loop:
    - choose the node with lowest distance to goad node from `frontier`
    - if `frontier` is empty or current node is goal node, break;
    - get current node's neighbours, for each neighbour:
        - if the neighbour node has not been visited yet: 
            - put the neighbour node into `frontier`

![greedy_best_first](images/a_star/best_first.gif)

The algorithm is really fast, but in complex map it may not find the shortest path, shown as below.
![in_complex_case](images/a_star/best_first_2.gif)

## A* Algorithm
Dijkstra’s Algorithm works well to find the shortest path, but it wastes time exploring in directions that aren’t promising. Greedy Best First Search explores in promising directions but it may not find the shortest path. The A* algorithm uses both the actual distance from the start and the estimated distance to the goal.

### Evaluation Function
In this algorithm, we use $g(n)$ to describe the cost from initial node to $n$ node, $h(n)$ describes the estimated cost between any node $n$ and the target node, and the total cost is:
$$
f(n) = g(n) + h(n)
$$

### Process
The `A* Algorithm` uses a `open lsit` and a `close list`, `open list` stores nodes haven't been calculated yet, and `close list` stores nodes have been searched. Every time we select a best node from `open list` and put it to `close list`, then we search the best node's neighbours, put the neighbour into `close list` if it is not in. Loop, until we find the goal node or there is no node in `open list`.

1. Put start node into `open list`
2. Loop:
    - Take the node A with lowest `F` from `open lsit`;
    - Put the node A into `close list`;
    - Search A's neighbours:
        - if the neighbour node is goal node, break;
        - if the neighbour node is unreachable or it's in `close list`, ignore it;
        - if the neighbour node is not in `open list`, put it into `open list`;
        - if the neighbour node is in `open list` but its new `G` is lower than its older, set its parent to A and reset its `G` and `H`
3. From the goal node link all nodes' parent, we get the best path.

### Heuristics
The heuristic function $h(n)$ tells A* an estimate of the minimum cost from any vetex $n$ to the goal, it can be used to control A*'s behavior.

- At one extreme, if $h(n)$ is 0, then only $g(n)$ plays a role, and A* turns into Dijkstra’s Algorithm, which is guaranteed to find a shortest path.
- If $h(n)$ is always lower than (or equal to) the cost of moving from $n$ to the goal, then A* is guaranteed to find a shortest path. The lower $h(n)$ is, the more node A* expands, making it slower.
- If $h(n)$ is exactly equal to the cost of moving from $n$ to the goal, then A* will only follow the best path and never expand anything else, making it very fast. Although you can’t make this happen in all cases, you can make it exact in some special cases. It’s nice to know that given perfect information, A* will behave perfectly.
- If $h(n)$ is sometimes greater than the cost of moving from $n$ to the goal, then A* is not guaranteed to find a shortest path, but it can run faster.
- At the other extreme, if $h(n)$ is very high relative to $g(n)$, then only $h(n)$ plays a role, and A* turns into Greedy Best-First-Search.


## Implementation

### Astar.h

```C++
// By yongcong.wang @ 16/07/2019
#ifndef A_STAR_H_
#define A_STAR_H_

#include <iostream>
#include <vector>
#include <list>
#include <math.h>
#include <limits>

namespace a_star {

struct Node {
  Node(int x, int y) : x(x), y(y), F(0), G(0), H(0), parent(nullptr) {}

  int x;
  int y;
  int F;
  int G;
  int H;

  Node *parent;
};

class Astar {
 public:
  Astar(std::vector<std::vector<int>> *maze);
  Astar();
  ~Astar();

  void setMaze(std::vector<std::vector<int>> *maze);
  Node *calcPath(Node *start_node, Node *end_node);
  void printPath(Node *header_node);

 private:
  Astar(const Astar &rhs);
  Astar &operator=(const Astar &rhs);

  const int straight_cost_;
  const int slope_cost_;

  std::vector<std::vector<int>> *maze_;

  int calcGcost(Node *curr_node, Node *next_node);
  int calcHcost(Node *curr_node, Node *end_node);
  int calcFcost(Node *node);
  bool isPassable(Node *curr_node, Node *next_node);
  bool isInList(std::list<Node *> &node_list, Node *node);
  bool isEqual(Node *first_node, Node *second_node);
  Node *getMinFcostNode(std::list<Node *> &node_list);
  std::list<Node *> getSurroundingNodes(Node *node);
};

}

#endif // A_STAR_H_

```

### Astar.cc

```C++
// By yongcong.wang @ 16/07/2019
#include "Astar.h"

namespace a_star {

Astar::Astar(std::vector<std::vector<int>> *maze) : 
    straight_cost_(10), slope_cost_(14), maze_(maze) {}

Astar::Astar() : straight_cost_(10), slope_cost_(14) {}

Astar::~Astar() {}

void Astar::setMaze(std::vector<std::vector<int>> *maze) {
  maze_ = maze;
}

Node *Astar::calcPath(Node *start_node, Node *end_node) {
  if (start_node == nullptr || end_node == nullptr) {
    std::cout << "calc Path: one of the nodes is not exist." << std::endl;
    return 0;
  }

  if (isEqual(start_node, end_node)) {
    return end_node;
  }

  std::list<Node *> open_list;
  std::list<Node *> close_list;
  open_list.push_back(start_node);

  while (!open_list.empty()) {
    Node *curr_node = getMinFcostNode(open_list);
    open_list.remove(curr_node);
    close_list.push_back(curr_node);
    std::list<Node *> surr_nodes = getSurroundingNodes(curr_node);

    for (auto next_node : surr_nodes) {
      if (isEqual(end_node, next_node)) {
        next_node->parent = curr_node;
        return next_node;
      }

      if (!isPassable(curr_node, next_node) ||
          isInList(close_list, next_node)) {
        continue;
      }

      if (!isInList(open_list, next_node)) {
        next_node->parent = curr_node;
        next_node->G = calcGcost(curr_node, next_node);
        next_node->H = calcHcost(next_node, end_node);
        next_node->F = calcFcost(next_node);
        open_list.push_back(next_node);
      } else {
        int new_g = calcGcost(curr_node, next_node);
        if (new_g < next_node->G) {
          next_node->parent = curr_node;
          next_node->G = new_g;
          next_node->H = calcHcost(next_node, end_node);
          next_node->F = calcFcost(next_node);
          open_list.push_back(next_node);
        }
      }
    }
  }

  return nullptr;
}

void Astar::printPath(Node *header_node) {
  if (header_node == nullptr) {
    return;
  }

  std::cout << "[" << header_node->x << ", " << header_node->y << "]" << std::endl;

  printPath(header_node->parent);
}

int Astar::calcGcost(Node *curr_node, Node *next_node) {
  if (curr_node == nullptr || next_node == nullptr) {
    std::cout << "G Cost: one of the nodes is not exist." << std::endl;
    return 0;
  }

  if (abs(curr_node->x - next_node->x) + abs(curr_node->y - next_node->y) == 1) {
    return curr_node->G + straight_cost_;
  } else {
    return curr_node->G + slope_cost_;
  }
}


int Astar::calcHcost(Node *curr_node, Node *end_node) {
  if (curr_node == nullptr || end_node == nullptr) {
    std::cout << "H Cost: one of the nodes is not exist." << std::endl;
    return 0;
  }

  return abs(pow(curr_node->x - end_node->x, 2) + pow(curr_node->y - end_node->y, 2));
}

int Astar::calcFcost(Node *node) {
  if (node == nullptr ) {
    std::cout << "F Cost: node is not exist." << std::endl;
    return 0;
  }

  return node->G + node->H;
}

bool Astar::isPassable(Node *curr_node, Node *next_node) {
  if (curr_node == nullptr || next_node == nullptr) {
    std::cout << "Reachable check: one of the nodes is not exist." << std::endl;
    return false;
  }

  if ((*maze_)[curr_node->x][curr_node->y] == 1 ||
      (*maze_)[next_node->x][next_node->y] == 1) {
    return false; // on obstacle
  }
  
  if ((*maze_)[curr_node->x][next_node->y] == 1 ||
      (*maze_)[next_node->x][curr_node->y] == 1) {
    return false; // on slope
  }

  return true;
}

bool Astar::isInList(std::list<Node *> &node_list, Node *node) {
  if (node_list.empty() || node == nullptr) {
    return false;
  }

  for (auto nd : node_list) {
    if (nd->x == node->x && nd->y == node->y) {
      return true;
    }
  }

  return false;
}

bool Astar::isEqual(Node *first_node, Node *second_node) {
  if (first_node == nullptr || second_node == nullptr) {
    std::cout << "Equal check: one of the inputs is not exist." << std::endl;
    return false;
  }

  return (first_node->x == second_node->x) && (first_node->y == second_node->y);
}

Node *Astar::getMinFcostNode(std::list<Node *> &node_list) {
  if (node_list.empty()) {
    std::cout << "Calc Min F Cost Node: node list is empty." << std::endl;
    return nullptr;
  }

  int min_f_cost = std::numeric_limits<int>::max();
  Node *min_node = nullptr;
  for (auto node : node_list) {
    if (min_f_cost > node->F) {
      min_f_cost = node->F;
      min_node = node;
    }
  }

  return min_node;
}

std::list<Node *> Astar::getSurroundingNodes(Node *node) {
  std::list<Node *> res;
  if (node == nullptr) {
    std::cout << "calc surrounding: input is not exist." << std::endl;
    return res;
  }

  for (int i = -1; i <= 1; ++i) {
    for (int j = -1; j <= 1; ++j) {
      if (i == 0 && j == 0) {
        continue;
      } else {
        res.push_back(new Node(node->x + i, node->y + j));
      }
    }
  }

  return res;
}

}

```

### main.cc

```C++
// By yongcong.wang @ 16/07/2019
#include "Astar.h"

int main() {
  std::vector<std::vector<int>> maze = {
    {1,1,1,1,1,1,1,1,1,1,1,1},
    {1,0,0,1,1,0,1,0,0,0,0,1},
    {1,0,0,1,1,0,0,0,0,0,0,1},
    {1,0,0,0,0,0,1,0,0,1,1,1},
    {1,1,1,0,0,0,0,0,1,1,0,1},
    {1,1,0,1,0,0,0,0,0,0,0,1},
    {1,0,1,0,0,0,0,1,0,0,0,1},
    {1,1,1,1,1,1,1,1,1,1,1,1}
  };

  a_star::Astar a(&maze);
  a_star::Node start_node(6, 4);
  a_star::Node end_node(6, 10);

  auto *header = a.calcPath(&start_node, &end_node);
  a.printPath(header);
}

```
### Build and Test

```Shell
g++ -std=c++11 -c Astar.cc && g++ -std=c++11 -c main.cc && g++ main.o Astar.o -o test && ./test
```

## Reference

- [Wikipedia: A* search algorithm](https://en.wikipedia.org/wiki/A*_search_algorithm)
- [Clear visual A* explanation, with advice and thoughts on path-finding](https://www.redblobgames.com/pathfinding/a-star/introduction.html)
- [路径规划之 A* 算法](https://paul.pub/a-star-algorithm/)
