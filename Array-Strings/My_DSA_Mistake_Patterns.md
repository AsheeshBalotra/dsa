# My DSA Mistake Patterns

The refined learning from my mistake bank, `My_DSA_Mistake_Bank.md` (22 problems as of 2026-11-18): how I think when I go wrong, and how to think instead. The bank keeps the raw evidence, and its "Root cause" column says which mistake here each logged category feeds. Refresh this file every 10 new problems, and whenever a mistake repeats.

---

## Three habits under everything

1. **Process, not property (mistakes 2, 3):** I code each step I picture ("at this corner, go down"), so the fact that stays true never gets written down. Instead: say what is always true before coding.
2. **Recognising, not checking (1, 2, 7):** a function's name, a familiar story or a half-remembered algorithm feels like understanding. Instead: treat recognition as a guess. Read the contract, compare the formulas, rebuild the idea.
3. **The judge decides (1, 4, 6):** "Accepted" ends my thinking, but the judge hid a wrong call (26), slow code (15, 347) and edge cases (8, 128, 523, 1010). Instead: before saying "done", dry-run a boundary and cost each line.

---

## How often I make each mistake

| # | Mistake                                                   | Problems                                 | Times | Type                         | Cost                       |
| - | --------------------------------------------------------- | ---------------------------------------- | ----- | ---------------------------- | -------------------------- |
| 1 | Trusting the name of a C++ call                           | 26, 33, 34, 128, 139, 380, 523, 560, 974 | 9     | implementation error (C++)   | wrong answer, often silent |
| 2 | Optimising the process instead of rewriting the condition | 11, 33, 49, 128, 347, 424, 560, 974      | 8     | conceptual gap               | no solution                |
| 3 | Coding every situation instead of the one rule            | 8, 26, 33, 53, 54, 56, 435               | 7     | implementation error         | bugs, extra code           |
| 4 | Judging cost by the shape of the code                     | 15, 26, 55, 128, 139, 347, 424           | 7     | conceptual gap (cost)        | wrong complexity           |
| 5 | Storing things before deciding what they mean             | 11, 73, 380, 424, 435, 523               | 6     | implementation error (state) | wrong answer               |
| 6 | Using the judge as my tester                              | 8, 33, 54, 128, 523, 1010                | 6     | edge-case oversight          | wrong answer, TLE, crash   |
| 7 | Reusing a familiar solution because the story matches     | 11, 435, 523, 560, 974                   | 5     | incorrect intuition          | wrong tool                 |

What the data says:
- **Implementation is the widest gap:** mistakes 1, 3 and 5 appear in 17 of the 22 problems. Mistake 2 costs the most: it is the only one that ends with no solution.
- **Weakest topic:** prefix sum + hash map. Its 4 problems (523, 560, 974, 1010) produced 10 of the 17 categories in the bank.
- **A written check is not enough:** `lower_bound` came back in 33 a day after I logged it in 34. A check only works if I run it.

---

## Mistake 1: Trusting the name of a C++ call

9 problems: 26, 33, 34, 128, 139, 380, 523, 560, 974. Wrong answers, often ones the judge misses.

**Mindset:** *"I know what this does."* I use a call from what its name suggests, not from its contract. The judge rarely catches it (26 passed only because Clang evaluates arguments left to right), so the wrong belief never gets corrected.

**What happened (33):**
- I believed *"if it is not in the array, `lb` points to `nums.end()`"*, so anything but `size` counted as found.
- `lower_bound` returns where the target would go: `[1]`, target 0 gives 0, which I reported as found.
- I passed `begin() + end` as the range, but ranges are `[first, last)`, so `nums[end]` was never searched: `[5,7,1,3]`, target 3 gave -1.

**Also:** 139 read `substr(i, 2)` as `s[i]` plus two more. 560, 974 and 523 used `if (m[key])`, which inserts the key, and a stored index 0 reads as false. 974 forgot that `%` can be negative, 26 put `i--` inside `swap(...)`, and 380 used `+ idx` on a set iterator.

**Think instead:** *"What exactly does it promise?"* Before relying on a call, say its three clauses: what it **returns** (also when the thing is missing), its **range** (is the end excluded?) and what it **changes** (inserts, shifts, copies). If unsure, try it on 2 elements.

Don't:
- Read `lower_bound`'s position as "found": check `lb < n && nums[lb] == target`.
- Ask `if (m[key])`: use `count` or `find`.
- Change `i` inside an expression that also reads it.

Code (33):
```cpp
return lb == n ? -1 : lb;                            // ✗ [1], target 0 -> 0
return (lb < n && nums[lb] == target) ? lb : -1;     // ✓ [1], target 0 -> -1
```

---

## Mistake 2: Optimising the process instead of rewriting the condition

8 problems: 11, 33, 49, 128, 347, 424, 560, 974. No solution without a hint.

**Mindset:** *"I have a way, I just need the trick that makes it fast."* I simulate the story (replace, extend, look back from each `i`) and try to speed that scan up, or try to recall the algorithm (49, 347). The fast solution comes from rewriting the condition, not the scan.

**What happened (424):**
- I fixed a target letter `s[i]` and a start `i`, then walked right paying k for the gaps: a simulation of the replacements.
- It missed `"BAAA"`, k = 1. The best window starts with the B that gets replaced, so no start found it: 3 instead of 4.
- It rescanned from every start: O(n²), about 10¹⁰ steps for n = 10⁵.
- The missing thought: "when is one substring good?" Keep its most frequent letter and replace the rest: `length - maxFreq <= k`. That test needs no target and no start, and it fits a sliding window.

**Think instead:** *"When is one candidate valid, and can I check that without a scan?"* Write the condition as a formula, then rewrite it in one of three ways: separate `i` and `j` (the inner loop becomes a lookup), find a fact true at every step (it rules something out), or find a test on one window. Check the rewrite on 2 or 3 numbers.

The rewrites from my problems:
```
brute-force condition                   rewritten                              tool
sum(j..i) == k                          prefix[j-1] == prefix[i] - k           count map (560)
sum(j..i) % k == 0                      prefix[j-1] % k == prefix[i] % k       remainder map (974)
is x the start of a run?                x - 1 is not in the set                count from starts (128)
can this substring become one letter?   length - maxFreq <= k                  sliding window (424)
which pair can never win?               any pair keeping the shorter line      two pointers (11)
```

Don't:
- Turn the story's operations into the algorithm.
- Speed up "for each start `i`, scan". Rewrite the condition instead.
- Fix the target or the start before the test. Let each candidate pick its own best.
- Wait for an algorithm to come back from memory. Rebuild it from the condition.

Code (424, full version in section 6 of `DSA_Techniques.md`):
```cpp
// ✗ the target is s[i] and the window starts at i: "BAAA", k = 1 -> 3
while (j < n && (s[j] == s[i] || used < k)) { if (s[j] != s[i]) used++; j++; }
// ✓ any window, with its own most frequent letter: "BAAA", k = 1 -> 4
while ((right - left + 1) - maxFreq > k) { count[s[left] - 'A']--; left++; }
```

---

## Mistake 3: Coding every situation instead of the one rule

7 problems: 8, 26, 33, 53, 54, 56, 435. Extra code and bugs, usually when I already had the idea.

**Mindset:** *"Handle each case as it comes."* Every moment I picture becomes a branch, and each branch feels thorough. In my words: *"I end up overcomplicating the solution by adding more if and else."*

**What happened (54):**
- I tracked `(i, j)` and wrote four "which corner am I at?" branches.
- With one row or column left, two branches were true at once, and the `||` in the loop condition ran past the last row into `matrix[-1]`. It took 31 minutes, with hints.
- The missing thought: the sides always come in the same order, so there is nothing to decide. Four loops in sequence, with two guards.

**Also:** 8 used flags for which part of the input I was in, and took 4 wrong submissions. 435 had three branches though I knew the rule. 33 added two shortcuts, both buggy. 56 kept checks the sort already guaranteed. 26 swapped, then repaired.

**Think instead:** *"What stays the same every round?"* Write the loop body as one "Repeat:" sentence, then:
- Same order every time: write the steps in sequence.
- Steps that differ only in numbers: put the numbers in an array.
- Bigger or smaller: `max` / `min`.
- More than 2 branches: find the rule first.

Don't:
- Ask "where am I?" (corner checks, flags) when the order of the steps is fixed.
- Add an `if` before running the main rule on that case.
- Patch a failing test with a special case. Ask why the rule failed.

Code (53; 54's before and after are in section 5 of `DSA_Approach_Framework.md`):
```cpp
if (currSum > 0 && currSum + nums[i] > 0) currSum += nums[i]; else currSum = nums[i];   // ✗ two conditions
currSum = max(nums[i], currSum + nums[i]);                                                // ✓ one rule
```

---

## Mistake 4: Judging cost by the shape of the code

7 problems: 15, 26, 55, 128, 139, 347, 424. The wrong complexity, which "Accepted" often hides.

**Mindset:** *"Two loops is n², one loop is n."* I cost the nesting I can see, not what each line does or how often it runs. And I let a structure clean up for me (a `set` to dedupe in 15, a `set` to sort the counts in 347, DP in 55) before asking what the input gives me for free.

**What happened (139):**
- My estimate: *"O(n² log n): n² for the iteration, log n for count"*.
- Without the memo, `solve(i)` repeats on every path. On `"a...ab"` with `["a", "aa", "aaa"]`, 10 `a`s took 600 calls and 20 `a`s took 266,079. That is exponential.
- `count` on a string key is not log n: `substr` copies the piece, and hashing reads all of it, so up to n per cut.
- With the memo I said O(n²), but it is n states × n cuts × O(n) per cut = O(n³).

139, costed properly:
```
version          calls                     cost per call                 total
no memo          the same i on every path  n cuts × n (substr + hash)    exponential
memo             n                         n cuts × n                    O(n³)
memo, cut ≤ L    n                         L cuts × L  (L: longest word) O(n · L²)
```

**Also:** 26 was one loop, but bubbling back with `i--` made it O(n²): 4 times the input took 16 times the steps. 128 was the opposite: a `while` inside a `for` is O(n) when each item is walked once. 424's per-start scan was never checked against n ≤ 10⁵.

**Think instead:** *"How many times does each line run in total, and what does one run cost?"*
- Read n first: 10⁵ needs O(n) or O(n log n).
- Per line: lookup O(1), set insert O(log n), erase in the middle O(n), `substr` or a string hash O(its length).
- Recursion: states × choices per state × cost per choice. Without a memo, count the paths that reach one state.
- Before adding a structure, ask what the input already gives: sorted order, bounded values, one block.

Don't:
- Read the cost off the loop nesting.
- Treat a library call as O(1) by default.
- Build extra work and clean it up afterwards (a `set` to dedupe).

---

## Mistake 5: Storing things before deciding what they mean

6 problems: 11, 73, 380, 424, 435, 523. Wrong answers when the stored value drifts from the plan.

**Mindset:** *"Store what I have, and use it later."* The whole interval because it has two numbers (435), every zero's cell (73), a count because 974 used one (523). I never write down what the value means, so nothing tells me when the code stops matching the plan.

**What happened (523):**
- The check `i - sumIdxMap[mod] >= 2` reads an index: where this remainder first appeared.
- The write, copied from 974, was `sumIdxMap[mod]++`: a count, changed on every visit.
- On `[5,0,0,0]`, k = 3, the "index" grew with `i`, so the check never passed: false instead of true.

**Also:** 11 planned "the farthest `j`" but stored the tallest. 73 stored every zero instead of which rows and columns. 380 updated a vector and a map separately, with no invariant written down. 424 couldn't say what the window should store.

**Think instead:** *"What will the later code ask?"* Write one sentence per structure: what its value means, and when it is written. Store only what is read. With two structures, write the invariant (`list[map[v]] == v`) and check it after every operation. For a window, write three lines: what the test reads, what changes when an element enters, and what changes when one leaves.

Don't:
- Pick a container before writing what its value means.
- Store something just because I have it.
- Copy a line that writes state from another problem without re-reading what the value means here.

Code (523):
```cpp
sumIdxMap[mod]++;                       // ✗ a count: [5,0,0,0], k = 3 -> false
if (!modExist) sumIdxMap[mod] = i;      // ✓ the first index, written once -> true
```

---

## Mistake 6: Using the judge as my tester

6 problems: 8, 33, 54, 128, 523, 1010. Failed submissions and a crash.

**Mindset:** *"It works on the example, so submit and see."* I test the typical case and let the judge find the rest. Boundaries sit at the ends of what the code computes, where I don't picture it running.

**What happened (1010):**
- My rule: two remainders that add up to 60, so look up `60 - r`. I pictured `r = 20` finding 40, and submitted.
- `r = 0` looks up 60, which is never a remainder, so `[60, 120]` gave 0 pairs. The partner of 0 is 0, not 60.

**Think instead:** *"Where are the ends, and what happens there?"* List the boundaries from the problem's shape, and dry-run one by hand:
```
where boundaries hide       ask                                   my miss
a computed key or index     can it reach k, n or -1?              1010: 60 - 0 = 60
the smallest inputs         0, 1 and 2 elements?                  33: 2 elements, mid == start
the last round of a loop    one row or column left?               54: || ran past the end
repeated values             does a repeat redo the work?          128: duplicate starts, TLE
index 0, value 0            can 0 be read as "missing"?           523: first index 0 read as false
each part of the input      missing? repeated? what ends it?      8: 4 wrong submissions
```

Don't:
- Submit after only the statement's example.
- Fix one failing case without checking the rest of its kind.

Code (1010):
```cpp
if (seen.count(60 - r)) ans += seen[60 - r];                  // ✗ r = 0 looks up 60: [60, 120] -> 0
if (seen.count((60 - r) % 60)) ans += seen[(60 - r) % 60];    // ✓ 60 becomes 0: [60, 120] -> 1
```

---

## Mistake 7: Reusing a familiar solution because the story matches

5 problems: 11, 435, 523, 560, 974. The wrong tool, then a wrong answer.

**Mindset:** *"I've seen this before."* Recognising the story feels like understanding the problem, so I reuse the old tool without comparing the formulas or reading the constraints.

**What happened (11):**
- "Two lines and water" looked like 42, so I built 42's tallest-wall arrays and paired each line with them.
- The formulas have different shapes (below): 42 adds up every column, while 11 scores one pair, where distance counts.
- On `[4,1,10,1,1,1,1,4]` both 4s paired with the nearby 10, giving 20. The answer is the two 4s at the ends: 28.

The two formulas:
```
42   sum over every column   min(leftMax[i], rightMax[i]) - h[i]   -> per-index maxima fit
11   best single pair        min(h[i], h[j]) * (j - i)              -> distance counts, maxima lose it
```

**Also:** 523 copied 974's `++`. 560 started as a sliding window, though negatives break it. 974 ignored that `%` can be negative. 435 missed the constraint `start < end`.

**Think instead:** *"Same story, but is it the same formula?"* Read the constraints first (sorted? negatives? how big is n?). Write both formulas side by side, then the one word that differs. In the prefix-sum family it is one word each:
```
560    prefix        -> count          seed {0: 1}
974    prefix % k    -> count          seed {0: 1}
523    prefix % k    -> first index    seed {0: -1}, length >= 2
1010   t % 60        -> count          look up (60 - r) % 60
```

Don't:
- Reuse a solution because the story sounds the same.
- Start a sliding window before checking for negative numbers.
- Skip the constraints.

Code (11):
```cpp
best = max({best, area(i, tallestLeft[i]), area(i, tallestRight[i])});   // ✗ 42's walls: [4,1,10,1,1,1,1,4] -> 20
best = max(best, min(h[lo], h[hi]) * (hi - lo));                          // ✓ two pointers from the widest pair,
if (h[lo] < h[hi]) lo++; else hi--;                                       //   dropping the shorter line -> 28
```

---

## Interview checklist

The number in brackets is the mistake each question prevents.

Before coding:
1. What does n allow? Sorted, negatives, duplicates? (4, 7)
2. When is one candidate valid? Rewrite that until it needs no scan, and test it on 2 or 3 numbers. (2)
3. Looks familiar? Same formula shape? What is the one word that differs? (7)
4. One sentence per structure: what its value means, and when it is written. (5)
5. What does each line cost, and how many times does it run in total? (4)
6. The loop body as one "Repeat:" sentence. More than 2 branches: find the rule. (3)

Before saying "done":
7. Every library call: what it returns when the thing is missing, its range, what it changes. (1)
8. Each special case: does the main rule already handle it? Then delete it. (3)
9. Dry-run one boundary by hand: key ranges, 0, 1 or 2 elements, the last round, repeats, index 0. (6)

After solving:
10. One Remember line, waste -> fix, then re-solve cold after 1 day, 1 week and 1 month. (2)
