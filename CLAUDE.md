# DSA Practice

SDE 2 DSA interview practice in C++. The goal is to learn every pattern and build the logic, not to solve many problems. Keep answers concise.

## Files

| File                                       | Purpose                                                                |
|--------------------------------------------|------------------------------------------------------------------------|
| `Array-Strings/Checklist_String_Array.md`  | what to solve, and in which order                                      |
| `Array-Strings/DSA_Approach_Framework.md`  | my framework for thinking through a problem to an optimal solution     |
| `Array-Strings/My_DSA_Mistake_Bank.md`     | the mistake bank: my logic and implementation mistakes, as raw data    |
| `Array-Strings/My_DSA_Mistake_Patterns.md` | what the bank teaches: ranked patterns, how to think, the checklist    |
| `Array-Strings/DSA_Techniques.md`          | named algorithms and techniques: what, why, where used, when it breaks |
| `Checklist_Other_Topics.md`                | what to solve in linked lists, trees, backtracking, DP and graphs      |
| `Graphs/`                                  | graph notes, a quick reference and a practice list                     |

The three `Array-Strings/` files work as one loop: the checklist picks the problem, the approach notes are how I work it out, and the mistake bank records what went wrong.

## Checklist

- **Next problem:** give only the LeetCode problem (number and name), the next one in the Order block. Never name its section or pattern.
- **At most two in a row from one section:** the first builds the concept, the second tests it. The third must come from another section.
- **After I solve it:** tell me which section (topic) it belongs to. If it uses an important algorithm or technique (Kadane, Boyer-Moore voting, monotonic stack, ...), give short context on it: what it is, why it works, and where else it is used. When I ask to keep it, add it as a new numbered section at the end of `DSA_Techniques.md`, in the format at the top of that file (name, used in, what it is, one example with a dry run), short, with its code compiled and outputs checked.
- **Solved on my own:** tick it and update the Progress count. Nothing else to record.
- **Needed help to get the answer** (a hint, fixes, or the solution shown to me): tick it as solved, update the Progress count, and add the label `[Needs revision]`. Never leave a problem unticked because I needed help.

## Approach notes

- `DSA_Approach_Framework.md` is a framework I built to think through problems. It rarely changes: edit it only when I ask. `Array-Strings/` holds the only copy.
- **When I am stuck:** give hints in levels (its section 8): the pattern name, then the state, then the code. The full solution only when I ask.

## Mistake bank and patterns

`My_DSA_Mistake_Bank.md` is the mistake bank: raw data, every mistake by problem, collected to find my recurring habits. `My_DSA_Mistake_Patterns.md` is the refined learning from it: the patterns ranked by how often I make them, how to think instead, and the short checklist I take into an interview. Both files show how often each mistake happens.

- **Record** logic going wrong (wrong approach or data structure, a missed observation or invariant, wrong assumptions about indexes, ranges, ordering or duplicates, a missed edge case or optimisation, needing substantial hints) and implementation errors (pointers, loop bounds, window or prefix state). Capture the reasoning gap, not only the wrong code and the fix.
- **Skip** simple syntax mistakes, and problems that were only slow to code but correct.
- **Do's and don'ts:** a short list of coding rules near the top of the bank, each with what it prevents. Add to it when I ask.
- **Format:** follow the rules at the top of the bank. Do not delete old entries.
- **Repeats:** when a new mistake matches a logged pattern, say so and in how many problems it has appeared. Call it a habit only at 3 or more problems, never from one. Update its count in both files: the category in the bank, and its mistake in `My_DSA_Mistake_Patterns.md` (the "Root cause" column of the bank's "Top mistakes" table says which).
- **Review** when I ask, or every 10 new problems: answer the five questions at the top of the bank, then refresh `My_DSA_Mistake_Patterns.md`: counts, ranking, what the data says, and the checklist.

## Links into cpp-notes

`cpp-notes` is a separate repo, next to this one at `../cpp-notes/`. Several files here point into it, so a change there can break them silently.

| File here                                 | Points to                                                                                                                        |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| `Array-Strings/My_DSA_Mistake_Bank.md`    | section numbers in `Notes_*.md` and `CplusNotes.md`                                                                              |
| `README.md`                               | the `cpp-notes` repo, and the section numbers of `CplusNotes.md`                                                                 |
| `Array-Strings/Checklist_String_Array.md` | `Notes_strings.md` and `Notes_bits.md`, by file name                                                                             |
| `Array-Strings/DSA_Approach_Framework.md` | `Notes_heap.md`, and the section "Which remainder to look for" in `Notes_math.md`                                                |
| `Array-Strings/DSA_Techniques.md`         | sections 10 and 11 of `CplusNotes.md`, sections 4 and 5 of `Notes_bits.md`, and "Which remainder to look for" in `Notes_math.md` |

- **When a `cpp-notes` section is renumbered, renamed or moved,** update every link above to match. Search this repo for the file name first: `grep -rn "Notes_maps" .`
- **The other way round:** `cpp-notes/CLAUDE.md` points at `Array-Strings/Checklist_String_Array.md` and `Array-Strings/My_DSA_Mistake_Bank.md`. Do not rename or move those two files without updating it.
- **When I ask for a fix to go into `cpp-notes`,** link it from the mistake bank by section number, and check that the section exists.

## When I share a solution

1. Check only what I ask. If I ask about one input, trace only that input. Test against a brute force or other inputs only when I ask, and give its time and space complexity. Compile with `clang++ -std=c++20` (no `<bits/stdc++.h>` on macOS), with test code outside this repo.
2. Explain what was wrong with the smallest input that breaks it, and a dry-run table when the logic is new to me.
3. Record what is worth recording, update the checklist, and give the next problem.
