# DSA Practice

SDE 2 DSA interview practice in C++. The goal is to learn every pattern and build the logic, not to solve many problems. Keep answers concise.

## Files

| File                                      | Purpose                                                            |
|-------------------------------------------|--------------------------------------------------------------------|
| `Array-Strings/Checklist_String_Array.md` | what to solve, and in which order                                  |
| `Array-Strings/ApproachForDSA.md`         | my framework for thinking through a problem to an optimal solution |
| `Array-Strings/My_DSA_Mistakes.md`        | my logic and implementation mistakes, as data to find my habits    |
| `Checklist_Other_Topics.md`               | what to solve in linked lists, trees, backtracking, DP and graphs  |
| `Graphs/`                                 | graph notes, a quick reference and a practice list                 |

The three `Array-Strings/` files work as one loop: the checklist picks the problem, the approach notes are how I work it out, and the mistakes log records what went wrong.

## Checklist

- **Next problem:** give only the LeetCode problem (number and name), the next one in the Order block. Never name its section or pattern.
- **At most two in a row from one section:** the first builds the concept, the second tests it. The third must come from another section.
- **Solved on my own:** tick it and update the Progress count. Nothing else to record.
- **Better solution shown to me:** leave it unticked, so it comes back later in the order.

## Approach notes

- `ApproachForDSA.md` is a framework I built to think through problems. It rarely changes: edit it only when I ask. `Array-Strings/` holds the only copy.
- **When I am stuck:** give hints in levels (its section 8): the pattern name, then the state, then the code. The full solution only when I ask.

## Mistakes log

`My_DSA_Mistakes.md` is a dataset for finding my recurring habits before interviews. Its "Before I code, before I submit" list is what I take into an interview.

- **Record** logic going wrong (wrong approach or data structure, a missed observation or invariant, wrong assumptions about indexes, ranges, ordering or duplicates, a missed edge case or optimisation, needing substantial hints) and implementation errors (pointers, loop bounds, window or prefix state). Capture the reasoning gap, not only the wrong code and the fix.
- **Skip** simple syntax mistakes.
- **Format:** follow the rules at the top of the file. Do not delete old entries.
- **Repeats:** when a new mistake matches a logged pattern, say so and in how many problems it has appeared. Call it a habit only at 3 or more problems, never from one.
- **Review** when I ask, or every 10 new problems: answer the five questions at the top of the file.

## When I share a solution

1. Test it against a brute force on random inputs, and give its time and space complexity. Compile with `clang++ -std=c++20` (no `<bits/stdc++.h>` on macOS), with test code outside this repo.
2. Explain what was wrong with the smallest input that breaks it, and a dry-run table when the logic is new to me.
3. Record what is worth recording, update the checklist, and give the next problem.
