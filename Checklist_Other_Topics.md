# DSA Checklist: Other Topics (SDE 2)

Linked lists, trees, backtracking, DP and graphs. These are the 13 problems from a "25 questions that cover most interview patterns" list that are not string or array problems. The other 12 are all in [Checklist_String_Array.md](arrays-strings/Checklist_String_Array.md).

**List tags:** `B` = Blind 75, `N` = NeetCode 150, `T` = LeetCode Top Interview 150.
**Difficulty:** E / M / H.

**Progress:** 1 of 13 solved.

---

## 1. Linked list
Use when: the input is a linked list. Most tricks are rewiring `next` pointers, or slow and fast pointers.

- [ ] 206. Reverse Linked List (E) [B N]: `prev`, `curr`, `next`; point `curr->next` back to `prev`
- [ ] 141. Linked List Cycle (E) [B N T]: slow and fast pointers meet only if there is a cycle
- [ ] 21. Merge Two Sorted Lists (E) [B N T]: dummy head, attach the smaller node each step

## 2. Trees
Use when: the input is a tree. Most answers combine the results from the left and right subtrees (DFS). Level questions use a queue (BFS).

- [ ] 543. Diameter of Binary Tree (E) [N]: DFS returns the height; the longest path through a node = left height + right height
- [ ] 102. Binary Tree Level Order Traversal (M) [B N T]: BFS with a queue, pop `q.size()` nodes per level
- [ ] 236. Lowest Common Ancestor of a Binary Tree (M) [T]: if `p` and `q` come back from different sides, this node is the answer

## 3. Backtracking
Use when: "return all" subsets, combinations or permutations.

- [ ] 78. Subsets (M) [N]: for each element, take it or skip it
- [ ] 39. Combination Sum (M) [B N T]: take the same candidate again or move to the next, stop when the sum passes the target

## 4. Dynamic programming
Use when: "number of ways" or "min / max cost", and the answer for `i` is built from answers for smaller `i`.

- [ ] 70. Climbing Stairs (E) [B N T]: `dp[i] = dp[i-1] + dp[i-2]`
- [ ] 198. House Robber (M) [B N T]: `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`
- [ ] 322. Coin Change (M) [B N T]: `dp[a] = min(dp[a - coin] + 1)` over all coins, `-1` if unreachable

## 5. Graphs
Use when: grids, connections, dependencies. The full graph list is in [graph-todo.txt](graphs/graph-todo.txt).

- [x] 200. Number of Islands (M) [B N T]: DFS or BFS from each unvisited land cell, count how many times you start one
- [ ] 207. Course Schedule (M) [B N T]: cycle in a directed graph; Kahn's BFS with in-degrees, or DFS with 3 states
