# DSA

My data structures and algorithms notes from interview prep. All code is in C++.

## Where to start

| If you want to... | Go to |
|---|---|
| Refresh C++ and common patterns | [CplusNotes.txt](CplusNotes.txt) |
| Pick the next string or array problem to practise | [Checklist_String_Array.md](arrays-strings/Checklist_String_Array.md) |
| Pick the next linked list, tree, backtracking, DP or graph problem | [Checklist_Other_Topics.md](Checklist_Other_Topics.md) |
| See which coding mistakes I keep repeating | [My_DSA_Mistakes.md](My_DSA_Mistakes.md) |
| Understand how graph algorithms work | [graph-notes.txt](graphs/graph-notes.txt) |
| Find a graph template or problem type fast | [graphs-dsa-reference.md](graphs/graphs-dsa-reference.md) |
| Pick the next graph problem to practise | [graph-todo.txt](graphs/graph-todo.txt) |

## Repo structure

```
dsa/
├── CplusNotes.txt                C++ refresher and DSA patterns, 25 numbered sections
├── Checklist_Other_Topics.md     linked lists, trees, backtracking, DP, graphs
├── My_DSA_Mistakes.md            mistakes by problem, tagged with the pattern behind each
├── arrays-strings/
│   └── Checklist_String_Array.md core LeetCode problems by pattern, ticked when solved
└── graphs/
    ├── graph-notes.txt           Graph concepts and the reasoning behind them
    ├── graphs-dsa-reference.md   Quick lookup: problem types, templates, Union-Find
    └── graph-todo.txt            Practice checklist, grouped by priority
```

## C++ notes

[CplusNotes.txt](CplusNotes.txt) by topic. Many sections include common bugs and how to fix them.

| Topic | Sections |
|---|---|
| Compiling and language basics | 1 compilation pipeline, 3 boolean truthiness, 6 pointers vs references, 7 the `&` operator, 16 char vs int |
| Vectors, iterators and strings | 2 `[]` vs `.at()`, 4 `begin()` and `end()`, 5 iterator operations, 15 string reversal |
| Sorting and heaps | 20 `lower_bound` and `upper_bound`, 21 sort comparators, 22 priority queue and heap |
| Searching and loops | 8 binary search, 14 majority element, 17 loop off-by-one errors, 18 slow and fast pointers |
| Bit manipulation | 19 AND, OR, XOR, NOT, shifts |
| Recursion and DP | 10 dynamic programming, 11 recursion direction |
| Trees | 9 ancestors and LCA, 12 tree recursion framework, 13 leaf vs null, 23 inorder traversal and BST validation |
| Graphs | 24 mark visited at push time, 25 adjacency list vs matrix |

## Graph notes

**[graph-notes.txt](graphs/graph-notes.txt)** explains the concepts:

- Graph and tree definitions, edge and vertex formulas
- Space complexity of adjacency lists and matrices, and avoiding TLE
- BFS vs DFS, and when to use which
- DFS path signatures (Number of Distinct Islands)
- Cycle detection in undirected and directed graphs

**[graphs-dsa-reference.md](graphs/graphs-dsa-reference.md)** is for quick lookup:

- Quick reference card: common mistakes, pattern matching, code skeleton
- Graph problem categories and a practice list ordered by difficulty
- Union-Find implementation, and when to use it instead of DFS or BFS
- Templates for BFS, DFS and multi-source BFS
- Interview tips and notes from solved problems

**[graph-todo.txt](graphs/graph-todo.txt)** tracks practice: what's done, the remaining topics in three priority tiers, and a suggested order.
