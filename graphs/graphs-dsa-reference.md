# Graph Data Structures - Complete Reference

## Quick Reference Card 🎯

### Common Mistakes to Avoid
❌ **Don't mark visited on pop** - Mark when pushing to queue (prevents duplicates)  
❌ **Don't forget both directions** - Undirected graphs need edge added twice  
❌ **Don't confuse V and E** - V = vertices (nodes), E = edges (connections)  
❌ **Don't use adjacency matrix** - Unless dense graph or need O(1) edge lookup  
❌ **Don't forget to visit ALL nodes** - Component counting needs outer loop

### Fast Pattern Matching
| You See | → Use This | → Why |
|---------|------------|-------|
| "How many groups/components?" | DFS/BFS counter loop | Count new component starts |
| "Shortest path/distance?" | BFS level-order | BFS explores nearest first |
| "Can schedule/order?" | Topological Sort | Dependency ordering |
| "Is there a cycle?" | DFS recursion stack | Detect back edges |
| "Dynamic connectivity?" | Union-Find | Efficient group merging |
| "Multiple sources spread?" | Multi-source BFS | All sources in queue together |

### Code Skeleton (Copy-Paste Ready)
```cpp
// 1. Build adjacency list
vector<vector<int>> adj(n);
for(auto& e : edges) {
    adj[e[0]].push_back(e[1]);
    adj[e[1]].push_back(e[0]); // Remove for directed
}

// 2. BFS template
queue<int> q;
vector<bool> vis(n, false);
q.push(start);
vis[start] = true;

while(!q.empty()) {
    int u = q.front(); q.pop();
    for(int v : adj[u]) {
        if(!vis[v]) {
            vis[v] = true;  // Mark at push!
            q.push(v);
        }
    }
}
```

---

## Table of Contents
1. [Graph Problem Categories](#graph-problem-categories)
2. [Foundation-Building Problem List](#foundation-building-problem-list)
3. [Core Techniques](#core-techniques)
4. [Union-Find Data Structure](#union-find-data-structure)
5. [Common Patterns](#common-patterns)
6. [Interview Tips](#interview-tips)

---

## Graph Problem Categories

### 1. **Graph Traversal & Connectivity**
- **Connected Components** (count, find, check)
- **Reachability** (can you get from A to B?)
- **Path Finding** (does a path exist?)

**Examples:** Number of Islands, Number of Provinces, Find if Path Exists

### 2. **Shortest Path Problems**
- **Single Source Shortest Path** (BFS for unweighted, Dijkstra for weighted)
- **Multi-Source Shortest Path** (multiple starting points)
- **All Pairs Shortest Path** (Floyd-Warshall)

**Examples:** Rotting Oranges, Minimum Knight Moves, Word Ladder

### 3. **Cycle Detection & Topological Ordering**
- **Cycle Detection** (directed vs undirected)
- **Topological Sort** (ordering with dependencies)
- **Strongly Connected Components** (cycles in directed graphs)

**Examples:** Course Schedule, Course Schedule II, Alien Dictionary

### 4. **Tree Problems in Graphs**
- **Minimum Spanning Tree** (Kruskal's, Prim's)
- **Tree Construction** from graphs
- **LCA** (Lowest Common Ancestor) in trees

**Examples:** Min Cost to Connect All Points, Critical Connections

### 5. **Dynamic Connectivity (Union-Find)**
- **Merge/Group Operations** (joining components)
- **Redundancy Detection** (which edge creates cycle?)
- **Dynamic Graph Updates** (edges added over time)

**Examples:** Redundant Connection, Number of Operations to Make Network Connected

### 6. **Graph Coloring & Partitioning**
- **Bipartite Graphs** (2-coloring)
- **Graph Coloring** (assign colors to nodes/edges)
- **Graph Partitioning** (split into groups)

**Examples:** Is Graph Bipartite, Possible Bipartition

### 7. **Special Graph Structures**
- **DAG (Directed Acyclic Graph)** specific problems
- **Tree-based problems** (since trees are special graphs)
- **Grid/Matrix graphs** (2D array as graph)

**Examples:** Most tree problems, matrix traversal problems

---

## Foundation-Building Problem List

**Ordered to build up complexity gradually — each one introduces exactly one new idea on top of the last.**

### ✅ Tier 1 — Grid-based DFS/BFS (closest to what you already know from 01 Matrix)

1. **200. Number of Islands** — the quintessential starting point. Grid DFS/BFS, counting connected components of 1s. Almost identical mechanics to your 01 Matrix multi-source BFS, but now you're counting groups instead of computing distances.

2. **695. Max Area of Island** — same traversal as #1, but track and return the size of each component instead of just counting components.

3. **733. Flood Fill** — simplest possible DFS/BFS on a grid, good confidence-builder, very close to a warm-up.

### ✅ Tier 2 — Explicit graph (adjacency list) traversal

4. **547. Number of Provinces** ✅ **COMPLETED** — given directly as an adjacency MATRIX (isConnected[i][j]), count connected components. Good bridge from grid-thinking to graph-thinking.

5. **1971. Find if Path Exists in Graph** ✅ **COMPLETED** — build an adjacency list from an edge list yourself (first time you're not handed a ready-made grid), then just check reachability via DFS/BFS.

6. **323. Number of Connected Components in an Undirected Graph** — same as #4/#5 but explicitly asks for the connected-components count; great one to solve twice — once with DFS/BFS, once with Union-Find, to compare both approaches directly.

### Tier 3 — BFS specifically for shortest path / levels

7. **994. Rotting Oranges** — multi-source BFS, essentially the same shape as your 01 Matrix problem (multiple starting points, compute time/distance to reach everything).

8. **1197. Minimum Knight Moves** (optional, trickier) — BFS shortest path on an implicit/infinite grid rather than a given one.

### Tier 4 — Cycle detection (new concept)

9. **207. Course Schedule** — cycle detection in a DIRECTED graph (this is where directed vs undirected starts to matter — a "visited" array alone isn't enough, you need to track "currently in the recursion stack" too, since revisiting a node during the SAME DFS path signals a cycle).

10. **841. Keys and Rooms** — simpler directed-graph reachability question, good before tackling #9 if directed graphs feel unfamiliar.

### Tier 5 — Union-Find practice (directly prepares you for Accounts Merge)

11. **1319. Number of Operations to Make Network Connected** — clean, minimal Union-Find application.

12. **684. Redundant Connection** — Union-Find where you detect the EXACT edge that creates a cycle (a very natural fit for Union-Find's "if two nodes are already in the same group, this new edge is redundant" logic).

### After this list

**You'll be ready for 721. Accounts Merge** — by then you'll have done grid DFS/BFS, adjacency-list DFS/BFS, BFS shortest-path, cycle detection, and Union-Find individually, so combining Union-Find with the string/hashmap bookkeeping will be the only new piece left to figure out.

---

## Core Techniques

### The "Big 4" Techniques (solve 80% of problems)

| Technique | Use Case | Key Pattern | Time Complexity |
|-----------|----------|-------------|-----------------|
| **DFS/BFS** | Traversal, connectivity, reachability | Visit all reachable nodes | O(V + E) |
| **Union-Find** | Dynamic connectivity, grouping | Merge groups, check same group | O(α(n)) per operation |
| **Dijkstra/BFS** | Shortest paths | Find minimum distance/cost | O(V + E) for BFS |
| **Topological Sort** | Ordering with constraints | Process dependencies first | O(V + E) |

### Graph Representation

#### Adjacency List (Preferred)
```cpp
// Build adjacency list from edge list
vector<vector<int>> adjList(n);
for(const auto& edge : edges) {
    adjList[edge[0]].push_back(edge[1]);
    adjList[edge[1]].push_back(edge[0]); // For undirected graphs
}
```
- **Space:** O(V + E)
- **Time to check if edge exists:** O(degree)
- **Best for:** Most graph problems

#### Adjacency Matrix
```cpp
vector<vector<int>> adjMatrix(n, vector<int>(n, 0));
for(const auto& edge : edges) {
    adjMatrix[edge[0]][edge[1]] = 1;
    adjMatrix[edge[1]][edge[0]] = 1; // For undirected graphs
}
```
- **Space:** O(V²)
- **Time to check if edge exists:** O(1)
- **Best for:** Dense graphs, when you need fast edge lookup

---

## Union-Find Data Structure

Union-Find efficiently handles **dynamic connectivity** problems.

### Core Operations
1. **Union**: Merge two components/groups
2. **Find**: Check if two nodes belong to the same component/group

### Implementation
```cpp
class UnionFind {
public:
    vector<int> parent, rank;
    int components;
    
    UnionFind(int n) {
        parent.resize(n);
        rank.resize(n, 0);
        components = n;
        for(int i = 0; i < n; i++) {
            parent[i] = i; // Each node is its own parent initially
        }
    }
    
    int find(int x) {
        if(parent[x] != x) {
            parent[x] = find(parent[x]); // Path compression
        }
        return parent[x];
    }
    
    void unionNodes(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        
        if(rootX != rootY) {
            components--; // Merge two components into one
            // Union by rank
            if(rank[rootX] < rank[rootY]) {
                parent[rootX] = rootY;
            } else if(rank[rootX] > rank[rootY]) {
                parent[rootY] = rootX;
            } else {
                parent[rootY] = rootX;
                rank[rootX]++;
            }
        }
    }
    
    bool connected(int x, int y) {
        return find(x) == find(y);
    }
    
    int countComponents() {
        return components;
    }
};
```

### When to Use Union-Find vs DFS/BFS

| Problem Type | Use Union-Find | Use DFS/BFS |
|---|---|---|
| **Dynamic connectivity** (edges added over time) | ✅ Perfect | ❌ Inefficient |
| **Static graph traversal** | ❌ Overkill | ✅ Perfect |
| **Cycle detection** | ✅ Great | ✅ Also great |
| **Shortest path** | ❌ Can't do it | ✅ Perfect |
| **Connected components count** | ✅ Great | ✅ Also great |

---

## Common Patterns

### BFS Template (Shortest Path/Level Order)
```cpp
bool bfs(vector<vector<int>>& graph, int start, int target) {
    vector<bool> visited(graph.size(), false);
    queue<int> q;
    q.push(start);
    visited[start] = true;
    
    while(!q.empty()) {
        int current = q.front();
        q.pop();
        
        if(current == target) return true;
        
        for(int neighbor : graph[current]) {
            if(!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }
    return false;
}
```

### DFS Template (Connectivity/Components)
```cpp
void dfs(vector<vector<int>>& graph, int node, vector<bool>& visited) {
    visited[node] = true;
    
    for(int neighbor : graph[node]) {
        if(!visited[neighbor]) {
            dfs(graph, neighbor, visited);
        }
    }
}

// Count connected components
int countComponents(int n, vector<vector<int>>& edges) {
    vector<vector<int>> graph(n);
    for(auto& edge : edges) {
        graph[edge[0]].push_back(edge[1]);
        graph[edge[1]].push_back(edge[0]);
    }
    
    vector<bool> visited(n, false);
    int components = 0;
    
    for(int i = 0; i < n; i++) {
        if(!visited[i]) {
            components++;
            dfs(graph, i, visited);
        }
    }
    return components;
}
```

### Multi-Source BFS Template
```cpp
int multiBFS(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    queue<pair<int,int>> q;
    
    // Add all sources to queue
    for(int i = 0; i < m; i++) {
        for(int j = 0; j < n; j++) {
            if(grid[i][j] == SOURCE_VALUE) {
                q.push({i, j});
            }
        }
    }
    
    int time = 0;
    int dirs[4][2] = {{0,1},{0,-1},{1,0},{-1,0}};
    
    while(!q.empty()) {
        int size = q.size();
        for(int i = 0; i < size; i++) {
            auto [x, y] = q.front();
            q.pop();
            
            for(int d = 0; d < 4; d++) {
                int nx = x + dirs[d][0];
                int ny = y + dirs[d][1];
                
                if(nx >= 0 && nx < m && ny >= 0 && ny < n && 
                   grid[nx][ny] == TARGET_VALUE) {
                    grid[nx][ny] = NEW_VALUE;
                    q.push({nx, ny});
                }
            }
        }
        time++;
    }
    return time;
}
```

---

## Interview Tips

### Problem Recognition

When you see a graph problem, ask:

| Question | → Category | → Technique |
|----------|------------|-------------|
| "How many connected components?" | **Connectivity** | DFS/BFS or Union-Find |
| "Shortest path/distance/time?" | **Shortest Path** | BFS (unweighted) or Dijkstra |
| "Can we order/schedule these?" | **Topological Sort** | DFS + Stack or BFS + indegree |
| "Is there a cycle?" | **Cycle Detection** | DFS with recursion stack |
| "Are these in the same group?" | **Union-Find** | Union-Find data structure |
| "Can we color this graph?" | **Graph Coloring** | DFS/BFS with color tracking |
| "Find minimum cost to connect?" | **MST** | Kruskal's or Prim's |

### General Approach

1. **Always clarify**: Directed vs undirected? Weighted vs unweighted?
2. **Start with brute force**: Even if not optimal, show you understand the problem
3. **Choose representation**: Adjacency list vs matrix based on problem constraints
4. **Practice both DFS and BFS**: Some problems work better with one approach
5. **Draw examples**: Visualizing graphs helps immensely

### Time/Space Complexity Cheat Sheet

| Operation | Adjacency List | Adjacency Matrix |
|-----------|----------------|-------------------|
| **Space** | O(V + E) | O(V²) |
| **Add edge** | O(1) | O(1) |
| **Check if edge exists** | O(degree) | O(1) |
| **DFS/BFS traversal** | O(V + E) | O(V²) |

### Study Approach

1. **Code each problem from scratch** (don't just read solutions)
2. **Time yourself** after you understand each problem  
3. **Practice explaining your approach** out loud
4. **Try multiple approaches** where applicable (DFS vs BFS vs Union-Find)
5. **Focus on pattern recognition** - most problems are variations of core patterns

---

## Progress Tracker

### ✅ Completed Problems

#### 547. Number of Provinces (BFS with adjacency matrix)
**Date Completed:** [Track your completion]  
**Key Learnings:**
- Given adjacency matrix directly (isConnected[i][j])
- Count connected components by starting BFS from each unvisited node
- Pattern: Loop all nodes, increment counter on each new component discovery

#### 1971. Find if Path Exists in Graph (BFS with adjacency list)
**Date Completed:** Aug 10, 2026  
**Key Learnings:**
- Built adjacency list from edge list for the first time
- Undirected graph: Added edges in BOTH directions
- O(V + E) space complexity deeply understood
- BFS pattern: visited tracking at push time prevents duplicates
- Early termination when destination found

**Code Template:**
```cpp
// Build adjacency list
vector<vector<int>> adjList(n);
for(auto& edge : edges) {
    adjList[edge[0]].push_back(edge[1]);
    adjList[edge[1]].push_back(edge[0]); // Undirected
}

// BFS traversal
queue<int> q;
vector<bool> visited(n, false);
q.push(source);
visited[source] = true;

while(!q.empty()) {
    int curr = q.front();
    q.pop();
    if(curr == destination) return true;
    
    for(int neighbor : adjList[curr]) {
        if(!visited[neighbor]) {
            visited[neighbor] = true;  // Mark at push time!
            q.push(neighbor);
        }
    }
}
```

#### 1319. Number of Operations to Make Network Connected (BFS with adjacency list, substituted for premium 323)
**Date Completed:** Aug 18, 2026
**Key Learnings:**
- 323 is LeetCode Premium -- substituted 1319 as the free equivalent for component-counting practice
- Math: a connected graph on n nodes needs >= n-1 edges (spanning tree property); if
  connections.size() < n-1, immediately impossible -> return -1
- Answer once feasible = components - 1 (each extra cable can bridge exactly one pair of
  separate components; need components-1 merges to reach 1 component)
- Time complexity is O(V+E) NOT O(V^2) despite the nested for+while loop shape -- the
  visited[] array guarantees every node is pushed/popped exactly ONCE and every adjacency
  list entry is scanned exactly once, summed across ALL outer-loop iterations combined
  (not per-iteration). The outer for loop only discovers new unvisited starting points;
  total traversal work is bounded by the whole graph's size regardless of how many
  components exist.
- Style: check the `connections.size() < n-1` impossibility case BEFORE running BFS, to
  avoid wasted O(V+E) work when the answer is already known to be -1
- Union-Find is an equally valid O(V+E) alternative for this problem (not asymptotically
  faster, just a different implementation style) -- union directly on the raw edge list,
  no adjacency list needed at all, answer = uf.components - 1

### 🎯 Next Up
- [ ] **684. Redundant Connection** - Union-Find where you detect the EXACT edge creating a cycle. Natural fit for "if two nodes are already in the same group, this new edge is redundant" logic.
- [ ] **994. Rotting Oranges** - Multi-source BFS (Tier 3), same shape as 01 Matrix -- revisit if skipped ahead to Union-Find track first.
- [ ] **841. Keys and Rooms** / **207. Course Schedule** - Directed graph reachability + cycle detection (Tier 4) -- needs "currently in recursion stack" tracking, not just visited[].

### 📋 Upcoming Problems (In Order)

### 📝 Key Learnings

#### Graph Representation Deep Dive
**Adjacency List vs Matrix** - Understanding O(V+E) Space Complexity

**Why V + E (not V × E)?**
- V = Number of **Vertices** (nodes) - the `n` parameter in most problems
- E = Number of **Edges** (connections) - `edges.size()` in most problems
- **Addition, not multiplication** because:
  - Outer structure: V empty vectors (one per vertex)
  - Inner elements: Sum of all neighbor lists = 2E for undirected, E for directed
  - Example with V=5, E=6:
    ```
    adjList[0] = [1, 2]       // 2 elements
    adjList[1] = [0, 3]       // 2 elements  
    adjList[2] = [0, 3, 4]    // 3 elements
    adjList[3] = [1, 2, 4]    // 3 elements
    adjList[4] = [2, 3]       // 2 elements
    
    Total: 5 vectors (V) + 12 integers (2E) = 17 units
    Matrix would be: 5×5 = 25 units
    ```

**When Each Representation Wins:**
- **Adjacency List (99% of cases):**
  - Sparse graphs (E << V²) - most real-world graphs
  - LeetCode constraints like V ≤ 2×10⁵ would need 160GB for matrix!
  - Better BFS/DFS performance: O(V+E) vs O(V²)
  
- **Adjacency Matrix (rare):**
  - Dense graphs (E ≈ V²)
  - Need O(1) edge lookups frequently
  - Specific algorithms like Floyd-Warshall

#### Directed vs Undirected Graphs
**The ONLY difference is in adjacency list construction:**

```cpp
// Undirected: Add edge in BOTH directions
for(auto& edge : edges) {
    adjList[edge[0]].push_back(edge[1]);
    adjList[edge[1]].push_back(edge[0]);  // Bidirectional
}

// Directed: Add edge in ONE direction only
for(auto& edge : edges) {
    adjList[edge[0]].push_back(edge[1]);  // One-way only
}
```

**BFS/DFS traversal code stays IDENTICAL** - it just follows whatever edges exist.

**Space difference:**
- Undirected: O(V + 2E) - each edge stored twice
- Directed: O(V + E) - each edge stored once
- Both simplify to O(V + E) in Big-O notation

#### Critical BFS/DFS Patterns
- **Visited Array**: Mark at PUSH time (not pop) to avoid queue duplicates
- **Connected Components**: Loop through all nodes, increment counter for each unvisited start
- **Path Finding**: Early return when target found, visited prevents cycles
- **Multi-source BFS**: Add all starting points to queue simultaneously before main loop

#### Union-Find Comparison
- **Different paradigm**: Manages groups instead of traversing
- **When to use**: Dynamic connectivity, cycle detection, grouping operations
- **When NOT to use**: Shortest path problems (can't do it), simple traversal (overkill)
- **Time**: O(α(n)) ≈ O(1) per operation with path compression + union by rank

#### Pattern Recognition Framework
Most graph problems are variations of:
1. **Connectivity** → DFS/BFS or Union-Find
2. **Shortest Path** → BFS (unweighted) or Dijkstra (weighted)
3. **Cycle Detection** → DFS with recursion stack (directed) or DFS with parent tracking (undirected)
4. **Topological Sort** → DFS + stack or BFS + in-degree