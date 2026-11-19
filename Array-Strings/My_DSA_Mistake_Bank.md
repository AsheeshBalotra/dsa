# My DSA Mistake Bank

The raw data: every mistake and logic gap from my array and string problems, grouped by problem. What I learn from it, refined into ranked patterns with how to think instead, is in `My_DSA_Mistake_Patterns.md`.

**Goal:** by interview day, the "Before I code, before I submit" list below is a short set of checks built from my own repeated mistakes. In an interview no judge catches a bug and no hint unblocks me, so these checks have to catch my habits first.

**How:** collect data now, draw conclusions later. Each entry is evidence, and a review uses the entries to answer:
1. Which mistakes do I repeat? (the "Top mistakes" table)
2. Is the gap in the **logic** (finding the idea) or the **implementation** (turning the idea into C++)? (K and pattern L)
3. Which part of my approach fails before the code? (the T rows of "Top mistakes")
4. Which problem types do I need hints for? (the Result lines)
5. Am I improving? A habit counts as fixed when it does not appear in the 10 problems after its check was added. (the Result dates)

What to record:
- Every mistake, even when the code passed: a worse complexity than needed, code that works only on one compiler, a fix that had to be shown to me.
- Logic gaps as well as code bugs: a wrong first idea, a hint I needed, a problem I could not solve.
- The reasoning gap: what I was thinking and why it went wrong, plus what I actually wrote, not only the fix, so the habit behind it stays visible.
- Not simple syntax mistakes, such as a typo or a missing semicolon. Only logic and implementation errors.

**K** = I already knew the logic and still made the mistake while coding it. A coding mistake without K happened while I was still working out the idea.

**To add an entry:** under the problem's heading, write a **Result** line: solved alone, after a hint (which level: pattern name, state or code), or from the solution, and the date. Then for each mistake write what I wrote, why it was wrong, the fix, the pattern number, and K if I knew the logic. Add the problem number to its row in "Top mistakes" (and to its K column if flagged), update Times, and move the row up if it now outranks the rows above it. Add a snippet under the pattern if the mistake shows it well. A gap in how I approached the problem, rather than in the code, goes in a T row.

**Review:** every 10 new problems, answer the five questions above with numbers from the tables and the Result lines. Update "What the table shows so far", and turn any pattern seen in 3 or more problems into a check in "Before I code, before I submit". Then refresh `My_DSA_Mistake_Patterns.md`.

---

## Top mistakes

Every mistake type, most repeated first. On a tie, the one that costs more in an interview comes first: no solution, then a wrong answer, then a wrong tool or complexity, then extra code. Number rows are code patterns and T rows are thinking gaps; their details and checks are in "Patterns" and "Thinking gaps" below. These categories roll up into 7 root causes, the mistakes in `My_DSA_Mistake_Patterns.md`, ranked by how often they happen. The "Root cause" column gives each row's mistake number there. Where it lists two, the second covers part of the row: 4's "two records of one fact" and T7's window state both count under 5.

| #  | Mistake                                                             | Problems                            | Times | K: knew the logic | Cost                | Root cause |
| -- | ------------------------------------------------------------------- | ----------------------------------- | ----- | ----------------- | ------------------- | ---------- |
| L  | Logic: could not find the idea                                      | 49, 347, 128, 560, 974, 11, 33, 424 | 8     | n/a               | no solution         | 2          |
| T6 | Adding special cases instead of finding the one rule                | 26, 33, 56, 435, 53, 54, 8          | 7     | n/a               | extra code, bugs    | 3          |
| 1  | Assuming what a C++ call does                                       | 128, 380, 974, 34, 33, 139          | 6     |                   | wrong answer        | 1          |
| T2 | Not working out the cost before coding                              | 15, 347, 128, 26, 139, 424          | 6     | n/a               | wrong complexity    | 4          |
| 2  | Relying on truthiness, precedence or evaluation order               | 560, 974, 523, 26                   | 4     | 560, 523          | wrong answer        | 1          |
| T4 | Letting the judge find my bugs                                      | 1010, 523, 128, 8                   | 4     | n/a               | wrong answer        | 6          |
| 8  | Using a heavier structure or step than needed                       | 347, 15, 26, 55                     | 4     |                   | wrong complexity    | 4          |
| 4  | Copy vs the real object, mixing containers, two records of one fact | 128, 380, 974                       | 3     | 380               | wrong answer        | 1, 5       |
| 5  | Testing only typical inputs                                         | 1010, 128, 33                       | 3     | 1010, 128         | wrong answer        | 6          |
| 7  | Storing the wrong thing in the state                                | 523, 11, 73                         | 3     | 523, 11           | wrong answer        | 5          |
| T1 | Picking a tool before reading the constraints                       | 560, 974, 435                       | 3     | n/a               | wrong tool          | 7          |
| T5 | Remembering the code instead of the idea                            | 49, 347                             | 2     | n/a               | no solution         | 2          |
| 6  | Wrong maths when deriving the relation                              | 560, 974                            | 2     | 560               | wrong answer        | 2          |
| T3 | Reusing a similar problem's code without the difference             | 523, 11                             | 2     | n/a               | wrong answer        | 7          |
| T7 | Not spotting a variable-size sliding window behind the story        | 424                                 | 1     | n/a               | no solution         | 2, 5       |
| 3  | Updating one structure but not the linked one                       | 380                                 | 1     | 380               | wrong answer        | 5          |
| 9  | Wrong loop condition or bounds                                      | 54                                  | 1     |                   | wrong answer, crash | 6          |

What the table shows so far:
- T6 is my most repeated coding habit: 7 problems (26, 33, 56, 435, 53, 54, 8), and in most of them I had the idea. I code the process moment by moment ("if I am at this corner, ..."), so every moment becomes a branch. Item 8 of "Before I code, before I submit" prevents it (write the repeating unit first), and item 11 catches what is left (the simplify pass).
- Pattern 2 is a habit: `if (m[key])` in 560, 974 and 523, three problems in a row.
- Pattern 1 is never K. These are C++ facts I did not know, so writing them in the `../../cpp-notes/` notes fixes them. In 34 and 33, one day apart, I used the position `lower_bound` returns without checking `nums[lb] == target`. In 139, I thought `substr(i, 2)` takes `s[i]` plus two more characters.
- In L, 4 of the 8 (49, 128, 560, 974) needed a transform before the lookup worked: a sorted key, a sequence start, a prefix sum, a remainder. 11 and 33 needed a rule that holds at every step instead: 11 drops the shorter side, 33 keeps the sorted half. In 424 the idea was right, but tied to a start index, so every start rescanned the range.
- Patterns 2, 3, 5 and 7 are mostly K. I had the idea and slipped while coding. The checklist catches these.
- 15 and 26 are both sorted arrays with duplicates, and both times I made extra work (a set, swaps to the end) and cleaned it up afterwards. Sorted means duplicates are neighbours: skip or overwrite them as I go.

---

## Before I code, before I submit

Built from the patterns and thinking gaps below. Run through it on every problem. The short interview version, 10 checks, is at the end of `My_DSA_Mistake_Patterns.md`.

**Before coding**
1. Constraints: sorted? negatives? duplicates? value range? (T1)
2. Brute force as a formula, and its cost. Then the relation; test it on 2 or 3 numbers. (6, L)
3. Shape of the formula: a sum over every index (per-index arrays fit), or a best pair (after one check, what can I rule out?), or the longest / shortest contiguous piece, often with a budget k (a variable-size sliding window: the four questions in section 6 of `DSA_Techniques.md`). (L, T3, T7)
4. Looks like a problem I solved? Write the one thing that differs before reusing its code or its tool. (T3)
5. List every factor in the score. Does my idea keep all of them? Try a tiny input where they pull opposite ways. (5, 7)
6. Write in one sentence what each structure's value means, and when it changes. Check the code computes that sentence. Store only what the later steps read: in 435 only the end, in 73 only which rows and columns. (3, 7)
7. Write the cost of each operation, then the total. A pointer moving backward (`i--`) in a one-pass loop usually means O(n²). (T2, 8)
8. Write the loop body as one sentence before coding: "Repeat: ...". Describe the repeating unit, not the cases. Steps that always run in the same order: write them in sequence. Steps that differ only in numbers: put the numbers in an array. A choice of bigger or smaller: `max` / `min`. More than 2 branches in the sentence: find the rule first. Method: "Before you type" in section 5 of `DSA_Approach_Framework.md`; for text in a fixed format, the parts line in section 10. (T6: 54, 435, 53, 8)

**Before submitting** (in an interview: before saying "done")
9. Every lookup uses `count` or `find`. Every condition is a full comparison. No `i++` / `i--` inside an expression that also reads `i`. (2)
10. **`lower_bound` does not find the target.** It returns where the target would go: the next bigger element if the target is missing, `end()` only if everything is smaller. Always check `i < n && nums[i] == target`. (1: 34, 33)
11. Simplify pass, 2 minutes: for each condition, does something I already know (sorting, the loop bounds, the rule) guarantee it? Delete it. For each special case or early return, run the main loop on it in my head: if it works, delete it. (T6: 26, 33, 56)
12. Dry-run one edge case by hand: index 0, remainder 0, duplicates, negatives. (5, T4)
13. **`substr(i, len)` counts `s[i]` itself.** It returns `s[i .. i+len-1]`, so `substr(i, 2)` is two characters, not `s[i]` plus two more. `i + len` is one past the end, so a piece fits when `i + len <= n`. (1: 139)

---

## Do's and don'ts

Short coding rules from my mistakes, each with what it prevents.

Don't:
- **`i--` (or any step back) inside a `for` loop.** A `for` loop promises that `i` moves forward once per step. `i--` breaks that, so I have to track by hand what `i` means after each step. It leads to off-by-one errors, infinite loops, and often O(n²), because each step back redoes work. It usually means I am changing the container I loop over (56, an old version with `erase`) or have not pinned down what `i` means (26).

Do:
- **Use a write pointer that only moves forward** to remove or merge elements while looping (26, 56). If a step sometimes should not advance, use a `while` loop and do `i++` only in the branch that moves on.

---

## Patterns

Mistakes in the code. Counts and ranking are in "Top mistakes".

### 1. Assuming what a C++ call does

**Check:** what does the call return, what happens if the key is missing or the container is empty, and what else does it change?
```cpp
// 380: + idx needs a random-access iterator
auto it = s.begin() + idx;              // ✗ s is an unordered_set: does not compile
return list[rand() % list.size()];      // ✓ keep the values in a vector

// 380: erase in the middle shifts every later element
list.erase(list.begin() + freq[val]);   // ✗ later indexes in freq go stale, and it is O(n)
list[idx] = list.back(); list.pop_back();   // ✓ fill the gap from the end (then update freq)

// 128: map keys are read-only
it->first = val + 1;                    // ✗ does not compile

// 974: % keeps the sign of the left number
int req = totalSumTillI % k;            // ✗ -7 % 5 is -2, so -2 and 3 become different keys
int req = ((totalSumTillI % k) + k) % k;   // ✓ always 0..k-1 (section 1 of ../../cpp-notes/Notes_math.md)

// 139: substr's length counts s[i] itself (section 4 of ../../cpp-notes/Notes_strings.md)
s.substr(i, 2);                         // s[i] and s[i+1]: two characters, not s[i] plus two more
for (int cut = 1; i + cut <= n; cut++)  // ✓ i + cut is one past the piece's end, so <= n
```

### 2. Relying on truthiness, precedence or evaluation order

**Check:** write the full comparison. Never use a number, index or iterator as a yes / no. Never change a variable (`i++`, `i--`) inside an expression that also reads it.
```cpp
// 560, 974, 523: [] as "does the key exist?" (section 1 of ../../cpp-notes/Notes_maps.md)
if (sumIdxMap[mod] && i - sumIdxMap[mod] >= 2)        // ✗ inserts mod, and a stored index 0 reads as false
if (sumIdxMap.count(mod) && i - sumIdxMap[mod] >= 2)  // ✓

// Not made yet, from my C++ notes:
if (s.find("cat"))                      // ✗ false when found at index 0
if (s.find("cat") != string::npos)      // ✓

if (n & 1 == 0)                         // ✗ parsed as n & (1 == 0)
if ((n & 1) == 0)                       // ✓

// 26: i-- inside a call that also reads i. The order of a call's arguments is not fixed
swap(nums[i-1], nums[i--]);             // ✗ if nums[i--] runs first, nums[i-1] is the wrong slot
swap(nums[i-1], nums[i]); i--;          // ✓ change i in its own statement
```

### 3. Updating one structure but not the linked one

**Check:** write the invariant first. For each method, list what changes in every structure, then check the invariant after each one.
```cpp
// 380, invariant: every value in list is in freq, and list[freq[v]] == v
bool insert(int val) {
    list.push_back(val);
                                        // ✗ missing: freq[val] = list.size() - 1;
}
bool remove(int val) {
    // ... move the last element into val's slot, pop_back ...
                                        // ✗ missing: freq.erase(val);
}
```

### 4. Copy vs the real object, mixing containers, two records of one fact

**Check:** when reading from a container, is this a copy or the real slot? Is the iterator compared with the `end()` of the same container? Is there one source of truth? Do the check and the read use the same key?
```cpp
// 380: swap with a copy
int last = list.back();
swap(list[freq[val]], last);            // ✗ swaps with the copy: the vector's last slot is unchanged
swap(list[freq[val]], list.back());     // ✓ both arguments are real slots

// 380: back() ignores my own size counter
int last = list.back();                 // ✗ the physical last slot holds a removed value
int last = list[list_size - 1];         // ✓ (better: drop list_size and pop_back)

// 128: iterator compared with another container's end()
if (map.find(val + 1) != nums.end())    // ✗
if (mapping.count(val + 1))             // ✓

// 974: checked one key, then read another
if (prefixSumModulo[k - req]) ans += prefixSumModulo[req];   // ✗
ans += prefixSumModulo[req];                                  // ✓ one key (and see pattern 2 for the check)
```

### 5. Testing only typical inputs

**Check:** dry-run duplicates, empty or one element, boundary values (0, `k`, `INT_MIN`, `INT_MAX`), and for design problems remove then re-insert.
```cpp
// 1010: when r == 0 the lookup is 60, which is never a remainder (0..59)
hash.find(60 - req)                     // ✗ misses pairs of multiples of 60
count[(60 - r) % 60]                    // ✓ the outer % turns 60 into 0

// 128: duplicates repeat the same walk
for (int x : nums)                      // ✗ TLE when a start value appears many times
for (int x : presentNums)               // ✓ the set holds each value once
```

### 6. Wrong maths when deriving the relation

**Check:** before coding, put a 2 or 3 element example through every line of the derivation. A range sum is always a difference: `sum(j..i) = prefix[i] - prefix[j-1]`.
```cpp
// 974: + instead of -
(prefix[j-1] + prefix[i]) % k == 0      // ✗ led to looking up k - req and -req
(prefix[i] - prefix[j-1]) % k == 0      // ✓ so prefix[j-1] % k == prefix[i] % k: look up req

// 560: wrong start index
sum(j..i-1) == prefix[i-1] - prefix[j]       // ✗ also removes nums[j]
sum(j..i-1) == prefix[i-1] - prefix[j-1]     // ✓
```

### 7. Storing the wrong thing in the state

**Check:** write in words what the map's value means, for example "the first index where this remainder appeared". Then check that every write keeps that meaning: what is stored, and when it is written.
```cpp
// 523: copied the counting line from 974
sumIdxMap[mod]++;                       // ✗ a count, changed on every visit
if (!modExist) sumIdxMap[mod] = i;      // ✓ an index, written only the first time

// 11: my plan said "farthest j with h[j] >= h[i]", the array stored something else
tallestAtRight[i] = /* index of the tallest line in i..n-1 */;   // ✗ tallest, ignores distance
// ✓ two pointers (or suffixMax + binary search for the farthest j that is tall enough)

// 73: stored every zero's (row, col), then cleared a full row and column per zero
vector<pair<int, int>> cords;           // ✗ O(mn) space, O(mn·(m+n)) time when most cells are 0
vector<bool> rowZero(m), colZero(n);    // ✓ later steps only ask "is this row / column marked?"

// 11: scanning height[n-1-i], stored the loop counter
rIdx = i;                               // ✗ i is the counter
rIdx = n - 1 - i;                       // ✓ the position
```

### 8. Using a heavier structure or step than needed

**Check:** what does the input already give me (sorted order, values bounded by `n`)? Do I need everything in order, or only the top k or the unique ones? Must the values I would overwrite survive? If not, overwrite instead of swapping.
```cpp
// 15: removed duplicate triplets after building them
set<vector<int>> ans;                            // ✗ O(n² log n), and a vector built per match
if (i > 0 && nums[i] == nums[i - 1]) continue;   // ✓ sorted: skip equal neighbours (same for start)

// 347: sorted every count to take the top k
set<pair<int, int>> s;                  // ✗ O(m log m) for all m unique values
// ✓ min-heap of size k, O(n log k) (section 4 of ../../cpp-notes/Notes_heap.md), or bucket sort by count, O(n)

// 26: swapped duplicates to the end, which broke the sorted order, then bubbled back to repair it
swap(nums[i], nums[last--]);  /* ... */  swap(nums[i-1], nums[i]); i--;   // ✗ O(n²)
if (nums[i] != nums[k - 1]) nums[k++] = nums[i];   // ✓ sorted: duplicates are neighbours, overwrite
```

### 9. Wrong loop condition or bounds

**Check:** for a loop over shrinking ranges, it stops when **any** range is empty, so the condition joins the "still non-empty" tests with `&&`. A crash "addition of unsigned offset ... overflowed" means a negative index: find the bound that went below 0.
```cpp
// 54: kept looping after the rows ran out, then read matrix[-1]
while (row_start <= row_end || col_start <= col_end)   // ✗
while (row_start <= row_end && col_start <= col_end)   // ✓
```

### L. Logic: could not find the idea

**Check:** dry-run the brute force, list every result, and find the repeated or dominated work (section 7 of `DSA_Approach_Framework.md`). If `i` and `j` do not separate, transform first: prefix sum, remainder, sorted key (section 4). For a best pair: start where one factor is largest, then drop the candidate that provably can't improve.
```cpp
// 128: the idea I missed. x starts a sequence exactly when x - 1 is missing
if (!s.count(x - 1)) { /* count up from x */ }

// 560: turn the range into two prefix sums, then separate i from j
// sum(j..i) == k  ->  prefix[j-1] == prefix[i] - k  ->  count map of prefix sums

// 974: same remainder means the difference is divisible
// (prefix[i] - prefix[j-1]) % k == 0  ->  prefix[j-1] % k == prefix[i] % k  ->  count map of remainders

// 11: start at the widest pair. Any narrower pair that keeps the shorter line can't win, so drop it
if (height[low] < height[high]) low++; else high--;
```

---

## Thinking gaps

Patterns 1 to 8 are mistakes in the code. These are gaps in how I work on a problem before the code. Counts and ranking are in "Top mistakes".

### T1. Picking a tool before reading the constraints

- 560: my first skeleton declared `start`, `end` and `sum`, a sliding window. Negatives are allowed, so both window rules fail.
- 974: negatives are allowed, and I did not ask what `%` does with a negative number.
- 435: did not read `start_i < end_i`, so I wrote a branch for equal starts that the overlap check already covers.

**Check:** answer step 1 of `DSA_Approach_Framework.md` (sorted? negatives? duplicates? value range?) before naming a tool. Each answer rules tools in or out.

### T2. Not working out the cost before coding

- 15 and 347: a `set` insert is O(log n). I did not count it, so pattern 8 went unnoticed until I asked for the complexity.
- 128: thought a `while` inside a `for` must be O(n²), without counting the total steps.
- 26: the opposite mistake. An `i--` inside the loop moves a value back one slot at a time, like insertion sort, so it is O(n²). I did not count it.
- 139: counted only the loops, twice. Without the memo I said O(n² log n), but the recursion reaches the same `i` along many paths, which is exponential. With the memo I said O(n²), but each cut copies and hashes a substring of up to n characters, so it is O(n³).
- 424: designed a per-start scan without checking it against n ≤ 10⁵. It is O(n²), about 10¹⁰ steps.

**Check:** next to each step of the plan, write the cost of its operation (map lookup O(1), set insert O(log n), vector erase in the middle O(n), `substr` or hashing a string O(its length)), then the total. For recursion: distinct states × work per state, and without a memo, count how many paths reach the same state. In an interview I say this before coding.

### T3. Reusing a similar problem's code without the difference

- 523: copied `++` from 974. These problems are one family, and the differences are one word each:
```
560    prefix        -> count          seed {0: 1}
974    prefix % k    -> count          seed {0: 1}
523    prefix % k    -> first index    seed {0: -1}, length >= 2
1010   t % 60        -> count          look up (60 - r) % 60
```
- 11: "lines holding water" looked like 42, so I reused its tallest-on-each-side arrays. The formulas have different shapes:
```
42   sum over every column   min(leftMax[i], rightMax[i]) - h[i]   -> per-index max arrays fit
11   best single pair        min(h[i], h[j]) * (j - i)              -> distance matters, max arrays lose it
```

**Check:** when a problem looks like one I solved, write what differs (key, value, seed, extra condition, the shape of the formula) before reusing any code or tool.

### T4. Letting the judge find my bugs

- 1010, 523 and 128 each failed a submission (wrong answer or TLE) that a dry run of one edge case would have caught. In an interview there is no judge.
- 8: 4 wrong submissions. Listing the parts of the input, and for each part "missing? repeated? what ends it?", gives the edge cases before coding.

**Check:** before submitting, dry-run one case from the pattern 5 list by hand.

### T5. Remembering the code instead of the idea

- 49 and 347: I had solved them before but could not rebuild them, because I remembered the code and not why it works.

**Check:** after each solve, write one Remember line in waste -> fix form (section 8 of `DSA_Approach_Framework.md`), and re-solve cold from that line after 1 day, 1 week and 1 month.

### T6. Adding special cases instead of finding the one rule

- 26: swapped duplicates to the end, then added a branch to repair the order. The rule: `nums[0..k-1]` holds the unique values, so overwrite.
- 33: added `lower_bound` shortcuts for sorted ranges, and three branches. The rule: at any `mid`, one half is sorted. Both shortcuts had bugs.
- 54: tracked `i` and `j` and picked the direction with four "which corner am I at?" checks. When `row_start == row_end` or `col_start == col_end`, two checks are true at once. The fixed order of four loops per round needs no position checks.
- 53: `if (currSum > 0 && currSum + nums[i] > 0)`. The second condition is redundant: when `currSum > 0`, extending always beats restarting. The whole `if`/`else` is `currSum = max(x, currSum + x)`.
- 56: two conditions the code already guaranteed. After sorting, an equal start already means `next[0] <= curr[1]`, so `curr[0] == next[0] ||` is redundant. `if (n == 1) return intervals;` is handled by the loop. Harmless, but each extra condition is one more thing to get wrong under pressure.
- 8: one loop with `digitFound` and `signFound` flags, for an input whose parts always come in the same order (spaces, sign, digits). One step per part, in sequence, needs no flags.

**Check:** before coding, write the rule the loop keeps in one sentence. Before adding an `if` for a case, run the main rule on that case: it often already works. When a test fails, ask why the rule failed instead of adding an `if` for that input. More than 2 branches in a loop body, or a pointer moving backward, is the warning sign.

### T7. Not spotting a variable-size sliding window behind the problem's story

- 424: "replace at most k letters, return the longest substring of one letter" read as operations to simulate, so I fixed a target letter per start index and scanned right from each start: a brute force, O(n²). I did not see that the k is a budget, and that "can this substring be made one letter with k replacements?" is a test on one window: `length - maxFreq <= k`, which keeps both window rules.
- 424, after the pattern name: I still could not see what to store for the window or how to update it as it moves. The test reads the most frequent letter's count, so the state is `count[26]`. Entering is easy (one count goes up); leaving can lower the max, so rescan the 26 counts.

**Check:** when the answer is the longest or shortest substring or subarray, run the four questions in section 6 of `DSA_Techniques.md`: contiguous piece? the condition as a test on one window, with operations turned into a cost against the budget k? updatable in O(1)? both window rules? If my brute force is "for each start, extend right", that is the rescan a window removes. Then write the state as three lines, "reads / enters / leaves" ("State: what to keep, and how it changes" in the same section).

---

## 8. String to Integer (atoi) (M)

**Result:** solved alone, accepted after 4 wrong submissions, 2026-11-18.

**1. One loop with flags for parts that always come in the same order** (T6)
- Wrote: one `for` loop over the whole string, with `digitFound` and `signFound` to remember where I was, and a branch each for space, sign, digit and anything else. Then `str.substr(0, 11)`, and a second loop that builds the number from the right with `mul`.
- Why it is too much: the input is always spaces, then at most one sign, then digits, then anything (ignored). The flags only record which part I am in, and every combination of flags is a case to get right (a space after the sign, a sign after a digit). `substr(0, 11)` and the second loop exist only to dodge overflow.
- Simpler: one step per part, in order, on a shared `i`. Where I am in the code says which part I am in, so there are no flags and no `else`. A loop's condition failing is the "else": it moves on to the next part. Both versions side by side: the worked example in section 10 of `DSA_Approach_Framework.md`.
- Question that finds it: does the input come in parts with a fixed order? Write them as one line first, `spaces* [sign] digits* rest`, then code one step per part: a part that repeats (`*`) is a `while`, an optional part (`[ ]`) is an `if`.

**2. The judge found the edge cases** (T4)
- 4 wrong submissions before it was accepted.
- For each part of the input line, ask: missing? repeated? what ends it? That gives the list before coding. The list for 8 is in section 10 of `DSA_Approach_Framework.md`.

---

## 11. Container With Most Water (M)

**Result:** after a hint (the pattern name, two pointers), 2026-11-14.

**1. Reused the Rain Water tool: tallest line on each side** (pattern L, T3)
- Tried: like 42, built `tallestAtRight[i]` and `tallestAtLeft[i]` (the index of the tallest line on each side), then paired each `i` with those two lines.
- Wrong because: 42 adds up every column, and each column only needs the tallest wall on each side. 11 picks one pair, scored `min(h[i], h[j]) * (j - i)`. Any height above the shorter line adds nothing, and distance counts as much as height. "Tallest" ignores distance.
- Breaks on: `[4,1,10,1,1,1,1,4]`. Both 4s pair with the close 10, so it returns 20. The answer is the two 4s at the ends, 28. The LeetCode example `[1,8,6,2,5,4,8,3,7]` passes, so only an input where height and distance pull opposite ways shows it.
- Fix: two pointers. Start at the widest pair. Every other pair is narrower, so it needs a taller shorter line to win. Any pair that keeps the shorter line is capped at its height and is narrower, so it can't win. Drop the shorter line. Each step drops one line: O(n).

**2. My plan said "farthest", my code computed "tallest"** (pattern 7)
- Plan (in my notes): for each `i`, the farthest `j` with `h[j] >= h[i]`. This rule is correct.
- Code: stored the tallest `j`, which answers a different question.
- My idea can work: `suffixMax` never increases, so binary search for the largest `j` with `suffixMax[j] >= h[i]`, and the same from the left. O(n log n). Two pointers is the O(n) version.

**3. Stored the loop counter instead of the position** (pattern 7, K)
- Wrote: `rIdx = i;` while scanning `height[n-1-i]`.
- Wrong because: the position being scanned is `n-1-i`, so `tallestAtRight` held wrong indexes.
- Fix: `rIdx = n - 1 - i;`. Even with this fix, mistake 1 still gives 20 instead of 28.

---

## 15. 3Sum (M)

**1. Used a `set` to remove duplicate triplets** (pattern 8)
- Wrote: `set<vector<int>> ans;`, inserted every match, then copied the set into the result.
- Wrong because: it passes, but duplicates are built and then thrown away. Time is O(n² log n) instead of O(n²), and the set is a second copy of the answer. With `[0, 0, 0, 0, 0]`, every inner step builds the same `{0, 0, 0}`.
- Fix: the array is sorted, so equal values sit next to each other. Skip a repeated `nums[i]`, and after a match skip a repeated `nums[start]`. Then no duplicate is ever built, and no set is needed.

---

## 26. Remove Duplicates from Sorted Array (E)

**Result:** solved alone in O(n²), 2026-11-16. The O(n) solution was shown to me.

My code. It passed, and gave the right answer on 20,000 random sorted arrays:
```cpp
int n = nums.size(), i = 1, last = n-1;
while (i <= last) {
    if (nums[i-1] == nums[i]) swap(nums[i], nums[last--]);       // send the duplicate to the end
    else if (nums[i-1] > nums[i]) swap(nums[i-1], nums[i--]);    // bubble the swapped-in value back
    else i++;
}
return i;
```

**1. Swapped duplicates to the end, then repaired the order: O(n²)** (pattern 8, T2)
- Idea: when `nums[i]` repeats `nums[i-1]`, swap it with the last unchecked slot. The value that comes in from the end is out of order, so bubble it back with `i--` until the front is sorted again.
- Wrong because:
  - The array is sorted, so duplicates are already neighbours and nothing ever needs reordering. The swap from the end broke the sorted order, and the `i--` steps then spent work repairing it.
  - Only the first `k` slots are checked, so the values after them don't need to survive. Overwriting is enough; a swap is not needed.
  - `i--` moves a value back one slot at a time, like insertion sort. With every value appearing twice: n = 1,000 took 127k loop steps, n = 4,000 took 2.0M, n = 16,000 took 32M. 4× the input gives 16× the work: O(n²).
  - The meaning of `i` and `last` keeps changing as values are swapped in and bubbled back, so there is no one-sentence invariant to check the code against.
- Fix: read and write pointers, both moving only forward. `k` means "`nums[0..k-1]` holds the unique values seen so far, in order". O(n) time, O(1) space.
```cpp
int k = 1;
for (int i = 1; i < nums.size(); i++)
    if (nums[i] != nums[k - 1]) nums[k++] = nums[i];
return k;
```
- Questions that catch it: what does the input already give me (sorted)? What does the output need (only the first `k` slots)? Does any pointer move backward? Can I say in one sentence what each pointer means?

**2. `i--` inside a call that also reads `i`** (pattern 2)
- Wrote: `swap(nums[i-1], nums[i--]);`
- Wrong because: C++ does not fix the order in which a call's arguments are evaluated. Clang, which LeetCode uses, goes left to right, so it passed. If `nums[i--]` is evaluated first, `i` drops before `nums[i-1]` is read, and the wrong pair is swapped. Simulating that order, 12,228 of 20,000 random inputs gave a wrong answer. The judge cannot catch this, because its compiler happens to use the safe order.
- Fix: `swap(nums[i-1], nums[i]); i--;`. Change a variable in its own statement.

---

## 33. Search in Rotated Sorted Array (M)

**Result:** 50 minutes, past the 25-minute hint point. The second version passed 147 LeetCode tests. The two-branch solution was shown to me, 2026-11-17. With the fixes for mistakes 2 and 3, my own version was accepted.

My code: if the whole array is sorted, use `lower_bound`. Otherwise binary search, with three branches: the range `[start, end]` is sorted (use `lower_bound` on it), `mid` is in the left sorted part, or `mid` is in the right sorted part.

**1. Used `lower_bound`'s position without checking it is the target** (pattern 1)
- Wrote: `return lb == nums.size() ? -1 : lb;`, and `return lb;` in the sorted-range branch.
- Wrong because: `lower_bound` returns where the target would go, not where it is. `[1]`, target 0 returns 0, and `[5,7,1,3]`, target -1 returns 2.
- Fix: `return (lb < n && nums[lb] == target) ? lb : -1;`
- Repeat: the 34 entry, written the day before, already says to check `nums[lb] == target`.
- The second version fixed the first branch, but the sorted-range branch still returned `lb` unchecked.

**2. Left the last element out of the `lower_bound` range** (pattern 1)
- Wrote: `lower_bound(nums.begin() + start, nums.begin() + end, target)`, then `if (lb == end) return -1;`.
- Wrong because: STL ranges are `[first, last)`, so `nums[end]` is never searched, and `end` is a real index, not "not found". `[5,7,1,3]`, target 3 returns -1 instead of 3.
- Fix: pass `nums.begin() + end + 1`, or drop the shortcut (see the fix below).

**3. Strict `>` to find the sorted half, which fails when `mid == start`** (pattern 5)
- Wrote: `nums[mid] > nums[start] && nums[mid] > nums[end]` to decide that `[start, mid]` is sorted.
- Wrong because: with 2 elements left, `mid == start`, so `nums[mid] > nums[start]` is false and the code assumes `[mid, end]` is sorted when it is not. `[3,1]`, target 1 returns -1 instead of 1.
- Fix: `if (nums[start] <= nums[mid])` means `[start, mid]` is sorted. A single element counts as sorted.
- Check: dry-run binary search on a 2-element range, where `mid == start`.

**4. Missed the one rule that holds at every `mid`, so I added special cases** (pattern L)
- Wrote: three branches, plus `lower_bound` shortcuts for "the whole array is sorted" and "this range is sorted".
- Missing idea: at any `mid`, at least one half is sorted, and `nums[start] <= nums[mid]` says which. A range check only works on a sorted half, so: if the target is in the sorted half's range, keep it, else take the other half. A fully sorted array needs no special case.
- The shortcuts caused mistakes 1 and 2.
- Fix:
```cpp
if (nums[start] <= nums[mid]) {                          // [start, mid] is sorted
    if (nums[start] <= target && target < nums[mid]) end = mid - 1;
    else start = mid + 1;
} else {                                                 // [mid, end] is sorted
    if (nums[mid] < target && target <= nums[end]) start = mid + 1;
    else end = mid - 1;
}
```
- Question that finds it: a binary search step drops the half that cannot hold the target. Which half can I answer that for? Only a sorted one.

---

## 34. Find First and Last Position of Element in Sorted Array (M)

**Result:** solved alone with `lower_bound` and `upper_bound`, accepted, 2026-11-16. I was not sure what the two calls return.

**1. Not sure what `lower_bound` and `upper_bound` return** (pattern 1)
- Did not know: with several copies of the target, `lower_bound` returns the first copy (the first element `>= target`), and `upper_bound` returns one past the last copy (the first element `> target`). Also did not know that `it - nums.begin()` turns the iterator into a 0-based index.
- Why it matters: the answer is `[lower_bound, upper_bound - 1]`. Without knowing both for sure, I cannot tell whether the answer is off by one.
- Also: when the target is missing, `lower_bound` still returns a position (where the target would go, or `nums.size()`). Check `first < n && nums[first] == target` before using it.
- Fix: section 20 of `../../cpp-notes/CplusNotes.md` now has the duplicates example and the index check. In an interview, also be ready to write the lower bound binary search by hand, because 34 is often asked without the STL.

---

## 53. Maximum Subarray (M)

**Result:** solved alone, accepted, 2026-11-17.

**1. A second condition, and `if`/`else` where a `max` is enough** (T6, K)
- Wrote: `if (currSum > 0 && currSum + nums[i] > 0) currSum += nums[i]; else currSum = nums[i];`, with `currSum` and `maxsum` starting at `-1e6`.
- Why it is too much: when `currSum > 0`, `currSum + nums[i] > nums[i]`, so extending always beats restarting. Kadane needs one condition: extend if the running sum is positive, else restart. And "extend or restart, whichever is bigger" is a `max`.
- Simpler, with no magic start value:
```cpp
int currSum = 0, maxSum = nums[0];
for (int x : nums) {
    currSum = max(x, currSum + x);   // extend or restart, whichever is bigger
    maxSum = max(maxSum, currSum);
}
```
- Question that finds it: does this branch only pick the bigger or smaller of two values? Then write `max` or `min`.

---

## 54. Spiral Matrix (M)

**Result:** 31 minutes, with LeetCode's hints. Runtime error ("addition of unsigned offset ... overflowed", `matrix[-1]`), 2026-11-18. The `||` to `&&` fix was shown to me, then my code was accepted.

**1. `||` in the loop condition** (pattern 9)
- Wrote: `while (row_start <= row_end || col_start <= col_end)`.
- Wrong because: the spiral is done when either the rows or the columns run out. On `[[1,2,3],[4,5,6],[7,8,9]]` all 9 cells were pushed, then the loop went on (no rows left, columns left), pushed 4 and 1 again, and read `matrix[-1][0]`.
- Fix: `&&`.

**2. Direction picked from the position of `i` and `j`** (T6)
- Wrote: four branches, each checking which corner `(i, j)` is at, and resetting `i` and `j` after each side.
- Why it is too much: when `row_start == row_end` or `col_start == col_end`, two branches match and the first one wins, which is hard to reason about. Each round can run the four sides in a fixed order (left→right, top→bottom, right→left, bottom→top), shrinking one boundary after each, with `if (row_start <= row_end)` before right→left and `if (col_start <= col_end)` before bottom→top. No `i`, `j` or position checks.

---

## 55. Jump Game (M)

**Result:** solved alone with memoized recursion, then bottom-up DP, both accepted, 2026-11-18. O(n²) where an O(n) greedy works. The greedy was shown to me after hints (the state level).

**1. DP where a greedy is enough** (pattern 8)
- Wrote: `solve(idx)` tries every jump from 1 to `nums[idx]`, with a `dp` array to remember each index. O(n × max jump) time, which is O(n²), plus O(n) recursion depth.
- Why it is too much: the only question is whether the last index is reachable, and reachable indexes form one block `0..farthest`. One variable, the farthest index reachable so far, answers it in one pass: O(n) time, O(1) space.
- Question that finds it: does each subproblem need its own answer, or does one running value cover all of them? If one value covers them, try a greedy before DP.
- The missed observation: any jump can be made shorter, so the reachable indexes form one block `0..farthest`. `i > farthest` means a gap, so return false.
```cpp
int farthest = 0;                              // indexes 0..farthest are reachable
for (int i = 0; i < nums.size(); i++) {
    if (i > farthest) return false;            // a gap: i can never be reached
    farthest = max(farthest, i + nums[i]);     // standing on i extends the reach
}
return true;
```

---

## 56. Merge Intervals (M)

**Result:** solved alone, accepted, 2026-11-17.

**1. Conditions the code already guaranteed** (T6)
- Wrote: `if (curr[0] == intervals[i][0] || curr[1] >= intervals[i][0])`, and `if (n == 1) return intervals;` before the loop.
- Why they are redundant: after sorting, `intervals[i][0] >= curr[0]`. An equal start means `intervals[i][0] == curr[0] <= curr[1]`, so the second condition already covers it. With one interval, the loop never runs and `ans` holds that interval.
- Simpler: `if (intervals[i][0] <= curr[1])`, and no early return.
- An older version of mine used `erase` plus `i--` to merge in place: O(n²), because each `erase` shifts every later element. For O(1) extra space, use a write pointer as in 26: merge into `intervals[k]`, or copy to `intervals[++k]`, then `resize(k + 1)`.

---

## 73. Set Matrix Zeroes (M)

**Result:** solved alone, accepted, 2026-11-18. The O(1)-space follow-up: solved after a hint (reuse the first row and column).

**1. Stored every zero's position instead of which rows and columns** (pattern 7)
- Wrote: a `vector<pair<int, int>>` of every zero, then for each one cleared its whole row and column.
- Why it is too much: a cell becomes 0 when its row or its column has a zero, so the later step only reads "is this row marked?" and "is this column marked?". Storing every cell costs O(z) space, up to O(mn), and each zero rewrites m + n cells, so an all-zero matrix takes O(mn·(m+n)).
- Fix: `vector<bool> rowZero(m), colZero(n)`, then `if (rowZero[i] || colZero[j]) matrix[i][j] = 0`. O(mn) time, O(m + n) space.
- Question that finds it: what do the later steps read? Store only that (as in 435).

---

## 128. Longest Consecutive Sequence (M)

**1. Could not see how to find the start of a sequence** (pattern L)
- Tried: a map of sequence tops, extending a sequence when `val + 1` exists. Got stuck on whether a number fits at the start or the end of a sequence.
- Missing idea: `x` starts a sequence exactly when `x - 1` is not in the set. Only count up from starts, because every other walk is part of a longer one (dominated work).
- Why my approach steps did not help: the order of the array does not matter, so "what do I know about everything before `i`?" has no useful answer. See section 7 of `DSA_Approach_Framework.md`.

**2. Mixed up container names** (pattern 4)
- Wrote: `if (map.find(val + 1) != nums.end())`, with the map named `mapping`.
- Wrong because: `map` is not my variable, and an iterator must be compared with the `end()` of the same container.
- Fix: `if (mapping.count(val + 1))`.

**3. Tried to change a map key** (pattern 1)
- Wrote: `it->first = val + 1;`
- Wrong because: map keys are read-only. To change a key, erase the entry and insert a new one.

**4. TLE: looped over `nums` instead of the set** (pattern 5, K)
- Wrote: `for (auto x : nums)` with the `x - 1` start check.
- Wrong because: if a start value appears many times, every copy repeats the same full walk, which is O(n²).
- Fix: `for (int x : presentNums)`. The set holds each value once.

**5. Doubted the O(n) complexity** (pattern L)
- Thought: a `while` inside a `for` must be O(n²).
- Correct reasoning: count the `while` steps over the whole run. Each number is walked over once in total, so it is about 2n steps, which is O(n).

---

## 139. Word Break (M)

**Result:** plain recursion first, then memoized after a hint (the state: the "times called" table showed `solve(i)` recomputed for the same `i`), accepted, 2026-11-18.

**1. Thought `substr(i, len)` takes `s[i]` plus `len` more characters** (pattern 1)
- Thought: `s.substr(i, 2)` is `s[i]`, `s[i+1]` and `s[i+2]`, so `i + cut == n` would read past the end, and the loop bound should be `<`.
- Wrong because: `len` counts `s[i]` itself, so the piece is `s[i .. i+cut-1]`, and `i + cut` is one past its end. `i + cut == n` is the piece that ends on the last character. With `<`, `s = "a"`, `wordDict = ["a"]` returns false.
- Fix: the warning in section 4 of `../../cpp-notes/Notes_strings.md`, and item 13 of "Before I code, before I submit".

**2. Counted only the loops for the cost** (T2, pattern 1)
- Plain recursion, I said O(n² log n): n² for the loops and O(log n) for `count`. But `unordered_set::count` is a hash lookup, O(1) on average (O(log n) is `set`), plus O(length) to hash a string key. And `solve(i)` is reached along many paths: on `"aaab"` with `["a", "aa", "aaa"]`, `solve(3)` ran 4 times, doubling with each extra `a`, so it is exponential.
- With the memo, I said O(n²): n states × n cuts. But each cut copies and hashes a substring of up to n characters, so it is O(n³). `string s` passed by value also copies the string on every call.
- Fix: work per state includes the cost of each operation inside the loop, not only how many times it runs.

---

## 347. Top K Frequent Elements (M)

**1. Sorted every count to get the top k** (pattern 8)
- Wrote: counted with an `unordered_map`, put every `(count, value)` in a `set<pair<int, int>>`, then took k from the back with `rbegin`.
- Wrong because: it passes, but it sorts all `m` unique values, O(n + m log m), when only the top k are needed.
- Fix: a min-heap of size k, O(n log k), or bucket sort by count, O(n), because a count is at most `n`.

---

## 380. Insert Delete GetRandom O(1) (M)

**1. Used `+ idx` on a set iterator** (pattern 1)
- Wrote: `auto it = s.begin() + idx;` on an `unordered_set`.
- Wrong because: only random-access iterators (`vector`, `deque`, `string`) support `+ idx`. `next(s.begin(), idx)` compiles but is O(n).
- Fix: keep the values in a `vector`, so `list[rand() % n]` is O(1).

**2. Erased from the middle of the vector** (patterns 1, 3)
- Wrote: `list.erase(list.begin() + freq[val]);`
- Wrong because: `erase` shifts every later element left, so their indexes in the map go stale. It is also O(n).
- Fix: move the last element into the gap, update its index in the map, then `pop_back()`.

**3. Removed from the vector but not from the map** (pattern 3, K)
- Wrote: a `remove` with no `freq.erase(val)`.
- Wrong because: the value still looked present, so re-inserting it returned false and removing it again returned true.

**4. Kept a separate `list_size` and used `list.back()`** (pattern 4, K)
- Wrote: removed values left at the end of the vector, tracked with `list_size`, then `int last = list.back();`.
- Wrong because: `back()` ignores `list_size`. It returns the physical last slot, which holds a removed value after any remove. The last live element is `list[list_size - 1]`.
- Fix: no separate size. `pop_back()` the removed slot, so `list.size()` is the only size.

**5. Swapped with a local copy** (pattern 4, K)
- Wrote: `int last = list.back(); swap(list[freq[val]], last); swap(freq[val], freq[last]);`
- Wrong because: `last` is a copy, so the vector's last slot did not change. After the first `swap`, `last` held `val`, so the second `swap` swapped `freq[val]` with itself.
- Fix: swap the slots themselves, `swap(list[i], list[j])`, or assign `list[idx] = last`. See section 2 of `../../cpp-notes/Notes_vectors.md`.

**6. Forgot to store the index on insert** (pattern 3, K)
- Wrote: `insert` with `list.push_back(val);` but no `freq[val] = list.size() - 1;`.
- Wrong because: the map stayed empty, so duplicates were inserted and `remove` always returned false.
- Lesson for 3, 4 and 6: write the invariant ("every value in `list` is in the map, and `list[map[v]] == v`") and check it after each method.

---

## 424. Longest Repeating Character Replacement (M)

**Result:** not solved on the first try, after the hint at the pattern-name level (sliding window), 2026-11-18. Then the state was explained and the full solution written into section 6 of `DSA_Techniques.md`. Re-solve cold.

**1. Tied the target letter to the start index** (pattern L, T7)
- Tried: for each `i`, take `s[i]` as the target letter, then walk right through the next copies of `s[i]` (a map of letter -> positions), spending k on the letters between them, and the rest of k to the right.
- Wrong because: the best window can start with a letter that gets replaced. On `"BAAA"`, k = 1, the answer is 4 (replace the B), but no start `i` with target `s[i]` reaches it: the best is `"AAA"`, 3. And moving `i` to `i + 1` rescans almost the same range, which is the waste to remove.
- The right part: "make every letter in a range equal to one letter" is the idea. Anchoring the range to its start was the gap.
- Signal I missed: "longest substring" + "at most k operations". The k is a budget: rewrite the operations as a test on one window (`length - maxFreq <= k`), then check the two window rules. 1004 Max Consecutive Ones III is the simplest form of this disguise.

**2. Did not check the cost against n ≤ 10⁵** (T2)
- Each start can walk O(n) positions (`"AAAA...A"`), so O(n²), plus `erase(begin())` on a vector, O(size) each time. With n up to 10⁵, that is about 10¹⁰ steps.
- Fix: step 1 of `DSA_Approach_Framework.md` before designing: n = 10⁵ means O(n) or O(n log n).

---

## 435. Non-overlapping Intervals (M)

**Result:** solved alone, accepted, 2026-11-17. First thought it might be DP, but n up to 10⁵ ruled out O(n²).

**1. Three branches where one condition is enough** (T6)
- Wrote: a branch for an equal start (`ans++`, keep the smaller end), a branch for an overlap (keep the interval with the smaller end), and a branch for no overlap. Stored the whole interval in `currInt`.
- Why it is too much: intervals have `start < end`, so an equal start is always an overlap, and the overlap branch already keeps the smaller end. Only the end of `currInt` is ever used.
- Simpler: one `int end`. `if (intervals[i][0] < end) { ans++; end = min(end, intervals[i][1]); } else end = intervals[i][1];`
- I had the rule (on an overlap, keep the interval that ends first) and still wrote each case I thought of as its own branch. The equal-start case was already an overlap.
- Tracked the start because an interval has two numbers. After sorting by start, every later interval starts at or after the kept one, so the only check is `next.start < end` and the kept start is never read. With only the end stored, "switch to the interval that ends first" is just `end = min(end, next.end)`.
- Question that finds it: what do the later checks read? Store only that.
- Missed constraint (T1): `start_i < end_i`, so every interval has positive length. After sorting, an equal start gives `next.start == curr.start < curr.end`, which the overlap check `next.start < curr.end` already catches. Reading that constraint deletes the first branch.
- Standard form: sort by end, keep each interval whose start is `>= end` of the last kept one, answer = n − kept. One condition.

---

## 523. Continuous Subarray Sum (M)

**1. Stored a count instead of the first index** (pattern 7, K)
- Wrote: `sumIdxMap[mod]++;` on every step, a line carried over from the 974 counting solution.
- Wrong because: the check `i - sumIdxMap[mod] >= 2` needs the index where `mod` first appeared. `++` turned the value into a count and changed it on every visit. `[5, 0, 0, 0]`, `k = 3` returned false, missing `[0, 0]`.
- Fix: `if (!modExist) sumIdxMap[mod] = i;`. Store the index, and only the first time, so the subarray found is as long as possible.

**2. Used `[]` to check if the key exists** (pattern 2, K)
- Wrote: `if (sumIdxMap[mod] && i - sumIdxMap[mod] >= 2)`.
- Wrong because: `[]` inserts a missing key with value 0, and a real index 0 reads as false, so a remainder first seen at index 0 looks missing.
- Fix: `bool modExist = sumIdxMap.find(mod) != sumIdxMap.end();`. See section 1 of `../../cpp-notes/Notes_maps.md`.

---

## 560. Subarray Sum Equals K (M)

**1. Could not find the state** (pattern L)
- Tried: for each `i`, look back for a subarray `j..i-1` with sum `k - nums[i]`. Got stuck on how to know that without re-adding.
- Missing idea: a range sum is a difference of two prefix sums. `sum(j..i) == k` becomes `prefix[j-1] == prefix[i] - k`, so `i` and `j` separate and the inner loop becomes a lookup in a count map.

**2. Off by one in the derivation** (pattern 6, K)
- Wrote: `sum(j, i-1) = prefix[i-1] - prefix[j]`.
- Wrong because: subtracting `prefix[j]` also removes `nums[j]`. The code still passed because it did not use this line.
- Fix: `sum(j..i-1) = prefix[i-1] - prefix[j-1]`.

**3. Used `[]` to check if the key exists** (pattern 2, K)
- Wrote: `if (prefixSumsSoFar[sumTillI - k]) ans += prefixSumsSoFar[sumTillI - k];`
- Wrong because: every miss inserts a fake key with count 0. The answer was still right, because adding 0 changes nothing, but the map fills with junk entries.
- Fix: `auto it = prefixSumsSoFar.find(sumTillI - k); if (it != prefixSumsSoFar.end()) ans += it->second;`

---

## 974. Subarray Sums Divisible by K (M)

**1. Could not see the relation** (pattern L)
- Tried: prefix sums with a map of remainders, but did not know which remainder to look up.
- Missing idea: `sum(j..i)` is divisible by `k` exactly when `prefix[i]` and `prefix[j-1]` have the same remainder. So look up the current remainder itself, in a count map.

**2. Added the prefix sums instead of subtracting** (pattern 6)
- Wrote: `[sum(0, j-1) + sum(0, i)] % k = 0`, so `sum(0, j-1) % k = -sum(0, i) % k`. Also `sum(j, i)/k = 0`, with `/` where `%` was meant.
- Wrong because: a range sum is `prefix[i] - prefix[j-1]`. The `+` made me look up `k - req` and `-req` instead of `req`.
- Fix: `(prefix[i] - prefix[j-1]) % k == 0`, which means `prefix[j-1] % k == prefix[i] % k`.

**3. Did not handle a negative remainder** (pattern 1)
- Wrote: `int req = totalSumTillI % k;`
- Wrong because: C++ `%` keeps the sign of the left number. With negative numbers, `-2` and `3` (the same remainder for `k = 5`) become different keys.
- Fix: `int req = ((totalSumTillI % k) + k) % k;`. See section 1 of `../../cpp-notes/Notes_math.md`.

**4. Checked one key and read another** (patterns 2, 4)
- Wrote: `if (prefixSumModulo[k-req]) ans += prefixSumModulo[req];`
- Wrong because: the check looks at key `k - req`, but the count added is for `req`. The `[]` in the check also inserts a fake key.
- Fix: use one key for both, found with `find` or `count`.

---

## 1010. Pairs of Songs With Total Durations Divisible by 60 (M)

**1. Missed the remainder 0 case in the first submission** (pattern 5, K)
- Wrote: look up `60 - r` for every remainder `r`.
- Wrong because: for `r = 0`, `60 - 0 = 60`, which is never a remainder (`0..59`). Pairs of multiples of 60, like `[60, 120]`, were not counted.
- Fix: look up `(60 - r) % 60`. The outer `% 60` turns 60 into 0 and leaves 1 to 59 unchanged.
- Lesson: check the range of every computed key or index. If it can reach `k`, wrap it with `% k`.

---

## Couldn't solve (details not recorded yet)

- 49. Group Anagrams
- 347. Top K Frequent Elements (the first attempt; the working solution is above)

Next time I revisit them, write down what blocked me.
