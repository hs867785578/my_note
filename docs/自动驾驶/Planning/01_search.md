# Search-based Path Finding

## Graph Search Basis

### Configuration Space

Here are some definations:

- `Robot Configuration`: a specification of the positions of all points of the robot
- `Degree of Freedom(DOF)`: the minimum number $n$ of real-valued coordinates needed to represent the robot configuration
- `Configuration Space`: a n-dim space containing all possible robot configurations, denoted as `C-space`, and each robot pose is a **point** in the C-space

Because:

- robot has different shape and size
- Collision detection requires knowing the robot geometry, which is time consuming and hard

we choose to do planning in configuration space. A transformation is as below:

![wspace to cspace](images/search/w2c_space.png)

- Robot is represented by a point in C-space, e.g. position(a point in $\mathbb{R}^3$), pose(a point in $SO(3)$), etc.
- Obstacles need to be represented in configuration space(one-time work prior to motion planning), called configuration space obstacle, or C-obstacle
- $\text{C-space} = \text{C-obstacle} \cup \text{C-free}$
- The path planning is finding a path between start point $q_{start}$ and goal point $q_{goal}$ within $\text{C-free}$

The basic introduction for graph and search methods can be found in [Coding/Algorithm/Graph](../../Coding/Algorithm/08_graph.md).

### Heuristic Search

BFS(Breadth First Search) and DFS(Depth First Search) pick the next node off the frontiers based on which was "first in" or "last in", and Greedy Best First picks the "best" node according to some rule, called a `heuristic`.

A Heuristic is a `guess` of how close you are to the target. There are many kinds of heuristics for distance estimation:

- Euclidean Distance
- Manhattan Distance
- Diagonal Distance

## Dijkstra

Dijkstra expands/visits the node with the `cheapest accumulated cost g(n)`, where

- $g(n)$ is the current best estimates of the accumulated cost from the start state to node `n`;
- Update the accumulated costs $g(m)$ for all unexpanded neighbors `m` of node `n`
- A node that has been expanded/visited is guaranteed to have the smallest cost from the start state.

A work flow for dijkstra can be:

- Maintain a `priority queue` to store all the nodes to be expanded
- The priority queue is initialized with the start state $X_s$
- Assign $g(X_s) = 0$ and $g(n) = infinite$ for all other nodes in graph
- Loop:
    - If the queue is empty, return `False`, break;
    - Remove the node n with the lowest $g(n)$ from the priority queue
    - Mark node n as expanded
    - If the node n is the goal state, return `True`, break
    - For all unexpanded neighbors "m" of "n":
        - If $g(m) == infinite$
            - $g(m)=g(n) + C_{nm}$
            - Push node "m" into the queue
        - If $g(m) > g(n) + C_{nm}$
            - $g(m) = g(n) + C_{nm}$
- End Loop

### Pros and Cons

- Pros
    - Complete and optimal
- Cons
    - Can only see the cost accumulated so far, thus exploring next station in every `direction`
    - No information about goal location

## A*

The A star algorithm is a dijkstra with a heuristic. It expands the node with cheapest cost $f(n) = g(n) + h(n)$, where

- $g(n)$ is the current best estimates of the accumulated cost from the start state to node `n`
- $h(n)$ is the estimated least cost from the node `n` to goal state.

A general workflow is:

- Maintain a `priority queue` to store all the nodes to be expanded
- The priority queue is initialized with the start state $X_s$
- Assign $g(X_s) = 0$ and $g(n) = infinite$ for all other nodes in graph
- Loop:
    - If the queue is empty, return `False`, break;
    - Remove the node n with the lowest $f(n) = g(n) + h(n)$ from the priority queue
    - Mark node n as expanded
    - If the node n is the goal state, return `True`, break
    - For all unexpanded neighbors "m" of "n":
        - If $g(m) == infinite$
            - $g(m)=g(n) + C_{nm}$
            - Push node "m" into the queue
        - If $g(m) > g(n) + C_{nm}$
            - $g(m) = g(n) + C_{nm}$
- End Loop

### Optimality
`A*` has optimality only if:

$$
h(n) \le h_{real}(n)
$$

where $h(n)_{real}$ is the true least cost to goal from node "n".

- if $h(n) << h_{real}(n)$, then the solution is optimal, but the searching speed will be slow;
- if $h(n)  > h_{real}(n)$, the solution is not optimal, which means that the path found by `A*` may not be the shortest path;
- if $h(n)  = h_{real}(n)$, the solution is optimal and the speed is the fastest.

### Speed up `A*`

#### The best heuristic
Even if we have an optimal heuristic cost, there is still something to optimize. For example, if we use the $f(n) = g(n)$ as cost function, we can surely find the shortest path, but the speed is slow. The closer our heuristic cost is to the actual distance from node to goal, the less steps we go through to reach the goal.

![heuristic](images/search/heuristic.png)

#### Tie breaker

In a 2D path without any obstacles, many paths have the same $f(n)$ value. There is no differences among them which makes them explored by `A*` equally.

![tie](images/search/tie.png)

We can change the $f(n)$ value slightly to break the tie:

- Interfere $h$ slightly: $h = h * (1.0 + p)$, where $p < \frac{\text{minimum cost of one step}} {\text{expected maximum path cost}}$;
- when $f$ is the same, compares $h$;
- add deterministic random numbers to the heuristic or edge costs(A hash of the coordinates);
- prefer paths that are along the straight line from the starting point to the goal;
- other more...


## Jump Point Search

Jump point search is a systematic approach to solve the tie problem of `A*`. The core idea of JPS is to find the symmetry and break them.

![jps](images/search/jps.png)

JPS explores intelligently, becasue it always looks ahead and jump.

### Look ahead rule

![look ahead](images/search/ahead.png)

- Neighbor pruning:
    - gray node: inferior neighbors, when going to them, the path without x is cheaper, Discard.
    - white node: nutural neighbors.
    - we only need to consider natural neighbors when expand the search.
- Forced neighbors
    - There is obstacle adjacent to x.
    - Red nodes are forced neighbors.
    - A cheaper path from x's parent to them is blocked by obstacle.

#### Jumping rule
![jump](images/search/jump.png)

- Recursively apply straight pruning rule and identify y as a jump point successor of x. This node is interesting because it has a neighbor z that can not reached optimally except by a path that visit x then y.
- Recursively apply the diagonal pruning rulea and identify y as a jump point successor of x.
- Before each diagonal step we first recurse straight. Only if both straight recursions fail to identify a jmp point do we step diagonally again.
- Node w, a forced neighbor of x, is expanded as normal.

#### Conclusion

- Most time, especially in complex environments, JPS is better, but far away from alway. For example, a large map with not many obstacles, JPS may be slower than `A*`.
- JPS reduces the number of nodes in `open list`, but increase the number of status query.
- JPS' limitation: only applicable to uniform grid map.

## Practice
