# Graph Notes (DSA interviews)

Quick lookup for graph problems: how to spot the problem type, the representations, the BFS / DFS templates, Union-Find, and the problems I've solved. The concepts are explained in `graph-notes.txt`. In `cpp-notes`, section 24 of `CplusNotes.md` covers marking visited at push time, and section 25 compares the adjacency list and matrix.

**V and E:** V is the number of vertices (nodes), usually the `n` parameter. E is the number of edges (connections), usually `edges.size()`.

## 1. Spotting the problem type

Ask first: directed or undirected? Weighted or unweighted? Then match the question:

| The question asks                                   | Technique                                         | Why                                   |
|-----------------------------------------------------|---------------------------------------------------|---------------------------------------|
| How many groups / connected components?             | DFS / BFS counter loop, or Union-Find             | each new unvisited start is one group |
| Shortest path, distance or time?                    | BFS if unweighted, Dijkstra if weighted           | BFS reaches the nearest nodes first   |
| Several sources spreading at once?                  | multi-source BFS                                  | all sources go in the queue together  |
| Can these be ordered or scheduled?                  | topological sort (DFS + stack, or BFS + indegree) | dependencies come first               |
| Is there a cycle?                                   | DFS with a recursion stack                        | an edge back into the current path    |
| Are these in the same group? Edges added over time? | Union-Find                                        | merges groups cheaply                 |
| Can the graph be 2-coloured?                        | DFS / BFS that tracks colours                     | neighbours must get different colours |
| Minimum cost to connect everything?                 | minimum spanning tree: Kruskal or Prim            | cheapest edges that join all nodes    |

Cost of the main techniques:

| Technique        | Use for                               | Time                  |
|------------------|---------------------------------------|-----------------------|
| DFS / BFS        | traversal, connectivity, reachability | O(V + E)              |
| Union-Find       | dynamic connectivity, grouping        | O(α(n)) per operation |
| BFS / Dijkstra   | shortest paths                        | O(V + E) for BFS      |
| Topological sort | ordering with dependencies            | O(V + E)              |

**α(n):** the inverse Ackermann function. It grows so slowly that it is below 5 for any real input, so Union-Find operations are effectively O(1).

---

## 2. Problem categories

### Traversal and connectivity

- Connected components: count them, find them, or check whether two nodes share one.
- Reachability: can you get from A to B?
- Path finding: does a path exist?

Examples: Number of Islands, Number of Provinces, Find if Path Exists in Graph.

### Shortest path

- Single source: BFS when unweighted, Dijkstra when weighted.
- Multi-source: several starting points at once.
- All pairs: Floyd-Warshall.

Examples: Rotting Oranges, Minimum Knight Moves, Word Ladder.

### Cycles and ordering

- Cycle detection, which works differently in directed and undirected graphs.
- Topological sort: an order that respects dependencies.
- Strongly connected components: groups in a directed graph where every node can reach every other, so each group of two or more nodes contains a cycle.

Examples: Course Schedule, Course Schedule II, Alien Dictionary.

### Trees in graphs

- Minimum spanning tree: Kruskal, Prim.
- Building a tree from a graph.
- Lowest common ancestor in a tree.

Examples: Min Cost to Connect All Points, Critical Connections in a Network.

### Dynamic connectivity (Union-Find)

- Merging groups as edges join them.
- Finding the redundant edge: which edge creates a cycle?
- Edges added over time.

Examples: Redundant Connection, Number of Operations to Make Network Connected.

### Colouring and partitioning

- Bipartite graphs: can two colours cover it with no edge joining the same colour?
- General colouring of nodes or edges.
- Splitting nodes into groups.

Examples: Is Graph Bipartite?, Possible Bipartition.

### Special structures

- Directed acyclic graphs (no cycles, edges have a direction).
- Trees, which are graphs with no cycles.
- Grids: a 2D array where each cell is a node joined to its neighbours.

Examples: most tree problems, matrix traversal problems.

---

## 3. Practice order

Each problem adds one new idea to the one before it.

### Tier 1: grid DFS / BFS (close to 01 Matrix)

1. 200 Number of Islands: count connected groups of 1s with DFS or BFS on the grid. The same mechanics as my 01 Matrix multi-source BFS, but counting groups instead of distances.
2. 695 Max Area of Island: the same traversal, but return the size of each group instead of counting them.
3. 733 Flood Fill: the simplest DFS / BFS on a grid, a warm-up.

### Tier 2: traversal on an explicit graph

4. 547 Number of Provinces (done): the graph comes as an adjacency matrix, `isConnected[i][j]`. Count the connected components. A bridge from grids to graphs.
5. 1971 Find if Path Exists in Graph (done): the first time I build the adjacency list myself, from an edge list. Then check reachability with DFS or BFS.
6. 323 Number of Connected Components in an Undirected Graph: the same as 4 and 5, but asks for the count directly. Solve it twice, with DFS / BFS and with Union-Find, to compare them. It is LeetCode Premium, so I solved 1319 instead (Tier 5).

### Tier 3: BFS for shortest path and levels

7. 994 Rotting Oranges: multi-source BFS, the same shape as 01 Matrix. Several starting points, and the time to reach every cell.
8. 1197 Minimum Knight Moves (optional, harder): BFS shortest path on an infinite grid that is never given as input.

### Tier 4: cycle detection (new concept)

9. 207 Course Schedule: a cycle in a directed graph. A visited array alone is not enough here. Also track which nodes are on the current DFS path: reaching one of them again means a cycle.
10. 841 Keys and Rooms: simpler reachability on a directed graph. Do it before 9 if directed graphs feel new.

### Tier 5: Union-Find (prepares for Accounts Merge)

11. 1319 Number of Operations to Make Network Connected (done): a small, clean Union-Find problem.
12. 684 Redundant Connection: find the exact edge that creates a cycle. If both ends of an edge are already in the same group, that edge is redundant.

### After this list

721 Accounts Merge: by then I'll have done grid DFS / BFS, adjacency-list DFS / BFS, BFS shortest path, cycle detection and Union-Find separately. The only new piece is combining Union-Find with the string and hash map bookkeeping.

---

## 4. Representing a graph

Build an adjacency list from an edge list:
```cpp
vector<vector<int>> adj(n);
for (auto& e : edges) {
    adj[e[0]].push_back(e[1]);
    adj[e[1]].push_back(e[0]);   // undirected only: delete this line for a directed graph
}
```

- Directed and undirected graphs differ only in this loop. The BFS and DFS code stays the same: it follows whatever edges exist.

Build an adjacency matrix:
```cpp
vector<vector<int>> mat(n, vector<int>(n, 0));
for (auto& e : edges) {
    mat[e[0]][e[1]] = 1;
    mat[e[1]][e[0]] = 1;         // undirected only
}
```

| Operation               | Adjacency list | Adjacency matrix |
|-------------------------|----------------|------------------|
| Space                   | O(V + E)       | O(V²)            |
| Add an edge             | O(1)           | O(1)             |
| Check if an edge exists | O(degree)      | O(1)             |
| DFS / BFS traversal     | O(V + E)       | O(V²)            |

### Why the list is O(V + E), not O(V × E)

- The outer vector holds V lists, one per vertex.
- The lists together hold 2E entries for an undirected graph (each edge is stored at both ends), or E for a directed one.
- So the space is V + 2E, which is O(V + E) in both cases. It is a sum because each edge is stored once or twice, not once per vertex.

Example with V = 5, E = 6:
```
adj[0] = [1, 2]       // 2 entries
adj[1] = [0, 3]       // 2 entries
adj[2] = [0, 3, 4]    // 3 entries
adj[3] = [1, 2, 4]    // 3 entries
adj[4] = [2, 3]       // 2 entries

Total: 5 lists (V) + 12 entries (2E) = 17 units
Matrix: 5 × 5 = 25 units
```

### Which one to use

- **Adjacency list:** almost always. Most graphs are sparse (E much smaller than V²), and DFS / BFS costs O(V + E) instead of O(V²).
- **Adjacency matrix:** only for dense graphs (E close to V²), when you need many O(1) edge checks, or for algorithms built on it, such as Floyd-Warshall.

> [!WARNING]
> A matrix does not fit for typical LeetCode limits. With V up to 2 × 10⁵, a matrix of `int` needs 4 × 10¹⁰ cells × 4 bytes = 160 GB.

---

## 5. BFS and DFS templates

> [!WARNING]
> Mark a node visited when you push it, not when you pop it. Marking at pop lets the same node enter the queue several times before it is first popped.

BFS: is `target` reachable from `start`? Levels come out in order, so BFS also gives shortest paths in an unweighted graph:
```cpp
bool bfs(vector<vector<int>>& adj, int start, int target) {
    vector<bool> visited(adj.size(), false);
    queue<int> q;
    q.push(start);
    visited[start] = true;                  // mark at push

    while (!q.empty()) {
        int u = q.front();
        q.pop();
        if (u == target) return true;       // stop early
        for (int v : adj[u]) {
            if (!visited[v]) {
                visited[v] = true;          // mark at push
                q.push(v);
            }
        }
    }
    return false;
}
```

- The `visited` array also stops the search from going round a cycle forever.

DFS, and counting connected components with it:
```cpp
void dfs(vector<vector<int>>& adj, int u, vector<bool>& visited) {
    visited[u] = true;
    for (int v : adj[u])
        if (!visited[v]) dfs(adj, v, visited);
}

int countComponents(int n, vector<vector<int>>& edges) {
    vector<vector<int>> adj(n);
    for (auto& e : edges) {
        adj[e[0]].push_back(e[1]);
        adj[e[1]].push_back(e[0]);
    }
    vector<bool> visited(n, false);
    int components = 0;
    for (int i = 0; i < n; i++) {           // outer loop over every node
        if (!visited[i]) {
            components++;                   // each new start is a new group
            dfs(adj, i, visited);
        }
    }
    return components;
}
```

- **Trap:** without the outer loop over every node, you only visit the start's own component.
- **Trap:** in an undirected graph, add each edge in both directions, or some nodes become unreachable.

### Why the nested loops are still O(V + E)

The outer `for` loop with a BFS or DFS inside looks like O(V²), but it is O(V + E). The `visited` array means each node is pushed and popped once, and each adjacency list is scanned once, over all the outer iterations together. The outer loop only finds new starting points. The total work is bounded by the size of the graph, however many components it has.

### Multi-source BFS

Put every source in the queue before the loop starts. The BFS then grows from all of them at once, one level per round:
```cpp
// Returns the number of levels processed, counting the sources' own level.
// In 994 Rotting Oranges, the minutes are this minus 1: the last round finds nothing new.
// Handle the edge cases separately: no fresh oranges -> 0, fresh ones left over -> -1.
int multiSourceBFS(vector<vector<int>>& grid, int source, int target, int done) {
    int m = grid.size(), n = grid[0].size();
    queue<pair<int, int>> q;
    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++)
            if (grid[i][j] == source) q.push({i, j});

    int levels = 0;
    int dirs[4][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    while (!q.empty()) {
        int size = q.size();                // one level per round
        for (int k = 0; k < size; k++) {
            auto [x, y] = q.front();
            q.pop();
            for (auto& d : dirs) {
                int nx = x + d[0], ny = y + d[1];
                if (nx >= 0 && nx < m && ny >= 0 && ny < n && grid[nx][ny] == target) {
                    grid[nx][ny] = done;    // mark at push
                    q.push({nx, ny});
                }
            }
        }
        levels++;
    }
    return levels;
}
```

---

## 6. Union-Find

**Union-Find:** a structure that tracks which group each node belongs to, as edges join groups. Also called a disjoint set union.

- **Find:** returns the root of a node's group. Two nodes are in the same group when they have the same root.
- **Union:** merges the groups of two nodes.
- **Path compression:** `find` points each node it passes straight at the root, so later finds are shorter.
- **Union by rank:** the shorter tree goes under the taller one, so trees stay flat.

```cpp
class UnionFind {
public:
    vector<int> parent, rank;
    int components;

    UnionFind(int n) : parent(n), rank(n, 0), components(n) {
        for (int i = 0; i < n; i++) parent[i] = i;   // each node is its own root
    }

    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);   // path compression
        return parent[x];
    }

    void unionNodes(int x, int y) {
        int rx = find(x), ry = find(y);
        if (rx == ry) return;                // already in one group
        components--;                        // two groups become one
        if (rank[rx] < rank[ry]) parent[rx] = ry;          // union by rank
        else if (rank[rx] > rank[ry]) parent[ry] = rx;
        else { parent[ry] = rx; rank[rx]++; }
    }

    bool connected(int x, int y) { return find(x) == find(y); }
    int countComponents() { return components; }
};
```

Union-Find or DFS / BFS:

| Problem                                      | Union-Find       | DFS / BFS          |
|----------------------------------------------|------------------|--------------------|
| Dynamic connectivity (edges added over time) | the best fit     | slow: re-traverses |
| Traversing a fixed graph                     | more than needed | the best fit       |
| Cycle detection                              | works well       | works well         |
| Shortest path                                | cannot do it     | the best fit       |
| Counting connected components                | works well       | works well         |

---

## 7. Problems I solved

### 547 Number of Provinces (BFS on an adjacency matrix)

- The graph came as an adjacency matrix, `isConnected[i][j]`.
- Count components by starting a BFS from each unvisited node, and adding 1 to the counter each time.

### 1971 Find if Path Exists in Graph (BFS on an adjacency list)

Solved Aug 10, 2026.
- The first time I built an adjacency list from an edge list.
- Undirected graph: add each edge in both directions.
- Why the list takes O(V + E) space: section 4.
- Mark visited at push time, so no node enters the queue twice.
- Return as soon as the destination is found.

### 1319 Number of Operations to Make Network Connected (BFS on an adjacency list)

Solved Aug 18, 2026, in place of 323, which is LeetCode Premium.
- A connected graph on n nodes needs at least n - 1 edges (a spanning tree). If `connections.size() < n - 1`, return -1 at once.
- Check that case before running the BFS, to skip O(V + E) work when the answer is already -1.
- Otherwise the answer is components - 1: each spare cable joins two separate components, and components - 1 joins give one.
- Time is O(V + E), not O(V²), despite the `for` loop around the BFS: see "Why the nested loops are still O(V + E)" in section 5.
- Union-Find is an equally good O(V + E) alternative, not faster, just a different style. Union directly on the edge list, with no adjacency list, and return `uf.components - 1`.
