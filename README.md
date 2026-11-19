# DSA

My data structures and algorithms practice for SDE 2 interviews. All code is in C++. The goal is to learn every pattern and build the logic, not to solve many problems.

## Where to start

| If you want to...                                                  | Go to                                                                     |
|--------------------------------------------------------------------|---------------------------------------------------------------------------|
| Pick the next array or string problem                              | [Checklist_String_Array.md](Array-Strings/Checklist_String_Array.md)      |
| Work out a solution step by step                                   | [DSA_Approach_Framework.md](Array-Strings/DSA_Approach_Framework.md)      |
| See every mistake I logged, by problem (raw data)                  | [My_DSA_Mistake_Bank.md](Array-Strings/My_DSA_Mistake_Bank.md)            |
| Revise the patterns behind my mistakes before an interview         | [My_DSA_Mistake_Patterns.md](Array-Strings/My_DSA_Mistake_Patterns.md)    |
| Look up a named technique, such as Kadane's algorithm              | [DSA_Techniques.md](Array-Strings/DSA_Techniques.md)                      |
| Pick the next linked list, tree, backtracking, DP or graph problem | [Checklist_Other_Topics.md](Checklist_Other_Topics.md)                    |
| Understand how graph algorithms work                               | [graph-notes.txt](Graphs/graph-notes.txt)                                 |
| Find a graph template or problem type fast                         | [graphs-dsa-reference.md](Graphs/graphs-dsa-reference.md)                 |
| Pick the next graph problem                                        | [graph-todo.txt](Graphs/graph-todo.txt)                                   |
| Refresh C++ and the STL                                            | [cpp-notes](https://github.com/AsheeshBalotra/cpp-notes), a separate repo |

## How I practise

The three files in `Array-Strings/` work as one loop:
1. **The checklist picks the problem.** One core problem per idea, tagged with the lists it appears in (Blind 75, Grind 75, NeetCode 150, LeetCode Top Interview 150). The order takes at most two problems in a row from one section: the first builds the concept, the second tests it. Popular problems that repeat a core idea are kept as Extras.
2. **The approach notes are how I work it out:** constraints, brute force, the waste, the state, a dry run, and then "can it be better?".
3. **The mistake bank records what went wrong.** `My_DSA_Mistake_Bank.md` collects every logic and implementation mistake as raw data, tagged with the pattern behind each. Every 10 problems, `My_DSA_Mistake_Patterns.md` turns the bank into what I learn from it: the patterns ranked by how often they happen, how to think instead, and the short checklist I take into an interview.

## Repo structure

```
dsa/
├── CLAUDE.md                       rules for Claude Code sessions in this repo
├── Checklist_Other_Topics.md       linked lists, trees, backtracking, DP, graphs
├── Array-Strings/
│   ├── Checklist_String_Array.md   core array and string problems by pattern, the order, Extras
│   ├── DSA_Approach_Framework.md   the steps to work out a solution from scratch
│   ├── My_DSA_Mistake_Bank.md      the mistake bank: every mistake by problem, as raw data
│   ├── My_DSA_Mistake_Patterns.md  what the bank teaches: ranked patterns, how to think instead, the checklist
│   └── DSA_Techniques.md           named algorithms and techniques, such as Kadane's
└── Graphs/
    ├── graph-notes.txt             graph concepts and the reasoning behind them
    ├── graphs-dsa-reference.md     quick lookup: problem types, templates, Union-Find
    └── graph-todo.txt              practice checklist, grouped by priority
```

## C++ notes

The C++ and STL notes are in their own repo, [cpp-notes](https://github.com/AsheeshBalotra/cpp-notes). Its longer notes, [`CplusNotes.md`](https://github.com/AsheeshBalotra/cpp-notes/blob/main/CplusNotes.md), by topic:

| Topic                          | Sections                                                                                                            |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------|
| Compiling and language basics  | 1 compilation pipeline, 3 boolean truthiness, 6 pointers vs references, 7 the `&` operator, 16 char vs int          |
| Vectors, iterators and strings | 2 `[]` vs `.at()`, 4 `begin()` and `end()`, 5 iterator operations, 15 string reversal                               |
| Searching and loops            | 8 binary search, 14 majority element, 17 loop off-by-one, 18 slow and fast pointers                                 |
| Bits                           | 19 AND, OR, XOR, NOT and shifts: extra uses                                                                         |
| Recursion and DP               | 10 dynamic programming, 11 recursion direction                                                                      |
| Trees                          | 9 ancestors and lowest common ancestor, 12 tree recursion framework, 13 leaf vs null, 23 inorder and BST validation |
| Sorting and heaps              | 20 `lower_bound` and `upper_bound`, 21 sort comparators, 22 priority queue and heap                                 |
| Graphs                         | 24 BFS: mark visited at push time, 25 adjacency list vs matrix                                                      |

## Graph notes

[`graph-notes.txt`](Graphs/graph-notes.txt) explains the concepts:
- Graph and tree definitions, edge and vertex formulas
- Space complexity of adjacency lists and matrices, and avoiding TLE
- BFS vs DFS, and when to use which
- DFS path signatures (Number of Distinct Islands)
- Cycle detection in undirected and directed graphs

[`graphs-dsa-reference.md`](Graphs/graphs-dsa-reference.md) is for quick lookup:
- How to spot the problem type, and what each technique costs
- Graph problem categories and a practice list ordered by difficulty
- Adjacency list vs matrix, and why the list takes O(V + E) space
- Templates for BFS, DFS and multi-source BFS, with the common traps
- Union-Find implementation, and when to use it instead of DFS or BFS
- Notes from the problems I solved

[`graph-todo.txt`](Graphs/graph-todo.txt) tracks practice: what's done, the remaining topics in three priority tiers, and a suggested order.
