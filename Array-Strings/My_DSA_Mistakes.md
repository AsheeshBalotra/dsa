# My Mistakes (DSA)

Every mistake and logic gap from my array and string problems, grouped by problem.

**Goal:** by interview day, the "Before I code, before I submit" list below is a short set of checks built from my own repeated mistakes. In an interview no judge catches a bug and no hint unblocks me, so these checks have to catch my habits first.

**How:** collect data now, draw conclusions later. Each entry is evidence, and a review uses the entries to answer:
1. Which mistakes do I repeat? (the pattern table)
2. Is the gap in the **logic** (finding the idea) or the **implementation** (turning the idea into C++)? (K and pattern L)
3. Which part of my approach fails before the code? (the thinking gaps table)
4. Which problem types do I need hints for? (the Result lines)
5. Am I improving? A habit counts as fixed when it does not appear in the 10 problems after its check was added. (the Result dates)

What to record:
- Every mistake, even when the code passed: a worse complexity than needed, code that works only on one compiler, a fix that had to be shown to me.
- Logic gaps as well as code bugs: a wrong first idea, a hint I needed, a problem I could not solve.
- The reasoning gap: what I was thinking and why it went wrong, plus what I actually wrote, not only the fix, so the habit behind it stays visible.
- Not simple syntax mistakes, such as a typo or a missing semicolon. Only logic and implementation errors.

**K** = I already knew the logic and still made the mistake while coding it. A coding mistake without K happened while I was still working out the idea.

**To add an entry:** under the problem's heading, write a **Result** line: solved alone, after a hint (which level: pattern name, state or code), or from the solution, and the date. Then for each mistake write what I wrote, why it was wrong, the fix, the pattern number, and K if I knew the logic. Add the problem number to that pattern's row in the table below (and to its K column if flagged), and add a snippet under the pattern if the mistake shows it well. If the gap was in how I approached the problem rather than in the code, add it to the thinking gaps table instead.

**Review:** every 10 new problems, answer the five questions above with numbers from the tables and the Result lines. Update "What the table shows so far", and turn any pattern seen in 3 or more problems into a check in "Before I code, before I submit".

---

## Before I code, before I submit

Built from the patterns and thinking gaps below. Run through it on every problem.

**Before coding**
1. Constraints: sorted? negatives? duplicates? value range? (T1)
2. Brute force as a formula, and its cost. Then the relation; test it on 2 or 3 numbers. (6, L)
3. Shape of the formula: a sum over every index (per-index arrays fit), or a best pair (after one check, what can I rule out?). (L, T3)
4. Looks like a problem I solved? Write the one thing that differs before reusing its code or its tool. (T3)
5. List every factor in the score. Does my idea keep all of them? Try a tiny input where they pull opposite ways. (5, 7)
6. Write in one sentence what each structure's value means, and when it changes. Check the code computes that sentence. (3, 7)
7. Write the cost of each operation, then the total. A pointer moving backward (`i--`) in a one-pass loop usually means O(n²). (T2, 8)

**Before submitting** (in an interview: before saying "done")
8. Every lookup uses `count` or `find`. Every condition is a full comparison. No `i++` / `i--` inside an expression that also reads `i`. (2)
9. Dry-run one edge case by hand: index 0, remainder 0, duplicates, negatives. (5, T4)

---

## Patterns

| #   | Pattern                                                             | Problems                   | K: knew the logic |
| --- | ------------------------------------------------------------------- | -------------------------- | ----------------- |
| 1   | Assuming what a C++ call does                                       | 128, 380, 974              |                   |
| 2   | Relying on truthiness, precedence or evaluation order               | 560, 974, 523, 26          | 560, 523          |
| 3   | Updating one structure but not the linked one                       | 380                        | 380               |
| 4   | Copy vs the real object, mixing containers, two records of one fact | 128, 380, 974              | 380               |
| 5   | Testing only typical inputs                                         | 1010, 128                  | 1010, 128         |
| 6   | Wrong maths when deriving the relation                              | 560, 974                   | 560               |
| 7   | Storing the wrong thing in the state                                | 523, 11                    | 523, 11           |
| 8   | Using a heavier structure or step than needed                       | 347, 15, 26                |                   |
| L   | Logic: could not find the idea                                      | 49, 347, 128, 560, 974, 11 | n/a               |

**What the table shows so far:**
- Pattern 1 is never K. These are C++ facts I did not know, so writing them in the `../C++/` notes fixes them.
- Patterns 2, 3, 5 and 7 are mostly K. I had the idea and slipped while coding. The checklist above catches these.
- Pattern 2 is a habit: `if (m[key])` in 560, 974 and 523, three problems in a row.
- In L, 4 of the 6 (49, 128, 560, 974) needed a transform before the lookup worked: a sorted key, a sequence start, a prefix sum, a remainder.
- 11 needed elimination instead: after one check, prove a candidate can't do better and drop it.
- 15 and 26 are both sorted arrays with duplicates, and both times I made extra work (a set, swaps to the end) and cleaned it up afterwards. Sorted means duplicates are neighbours: skip or overwrite them as I go.

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
int req = ((totalSumTillI % k) + k) % k;   // ✓ always 0..k-1 (section 1 of ../C++/Notes_math.md)
```

### 2. Relying on truthiness, precedence or evaluation order

**Check:** write the full comparison. Never use a number, index or iterator as a yes / no. Never change a variable (`i++`, `i--`) inside an expression that also reads it.
```cpp
// 560, 974, 523: [] as "does the key exist?" (section 1 of ../C++/Notes_maps.md)
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
// ✓ min-heap of size k, O(n log k) (section 4 of ../C++/Notes_heap.md), or bucket sort by count, O(n)

// 26: swapped duplicates to the end, which broke the sorted order, then bubbled back to repair it
swap(nums[i], nums[last--]);  /* ... */  swap(nums[i-1], nums[i]); i--;   // ✗ O(n²)
if (nums[i] != nums[k - 1]) nums[k++] = nums[i];   // ✓ sorted: duplicates are neighbours, overwrite
```

### L. Logic: could not find the idea

**Check:** dry-run the brute force, list every result, and find the repeated or dominated work (section 7 of `../C++/ApproachForDSA.md`). If `i` and `j` do not separate, transform first: prefix sum, remainder, sorted key (section 4). For a best pair: start where one factor is largest, then drop the candidate that provably can't improve.
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

Patterns 1 to 8 are mistakes in the code. These are gaps in how I work on a problem before the code.

| #   | Gap                                                     | Seen in          |
| --- | ------------------------------------------------------- | ---------------- |
| T1  | Picking a tool before reading the constraints           | 560, 974         |
| T2  | Not working out the cost before coding                  | 15, 347, 128, 26 |
| T3  | Reusing a similar problem's code without the difference | 523, 11          |
| T4  | Letting the judge find my bugs                          | 1010, 523, 128   |
| T5  | Remembering the code instead of the idea                | 49, 347          |

### T1. Picking a tool before reading the constraints

- 560: my first skeleton declared `start`, `end` and `sum`, a sliding window. Negatives are allowed, so both window rules fail.
- 974: negatives are allowed, and I did not ask what `%` does with a negative number.

**Check:** answer step 1 of `../C++/ApproachForDSA.md` (sorted? negatives? duplicates? value range?) before naming a tool. Each answer rules tools in or out.

### T2. Not working out the cost before coding

- 15 and 347: a `set` insert is O(log n). I did not count it, so pattern 8 went unnoticed until I asked for the complexity.
- 128: thought a `while` inside a `for` must be O(n²), without counting the total steps.
- 26: the opposite mistake. An `i--` inside the loop moves a value back one slot at a time, like insertion sort, so it is O(n²). I did not count it.

**Check:** next to each step of the plan, write the cost of its operation (map lookup O(1), set insert O(log n), vector erase in the middle O(n)), then the total. In an interview I say this before coding.

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

**Check:** before submitting, dry-run one case from the pattern 5 list by hand.

### T5. Remembering the code instead of the idea

- 49 and 347: I had solved them before but could not rebuild them, because I remembered the code and not why it works.

**Check:** after each solve, write one Remember line in waste -> fix form (section 8 of `../C++/ApproachForDSA.md`), and re-solve cold from that line after 1 day, 1 week and 1 month.

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

## 128. Longest Consecutive Sequence (M)

**1. Could not see how to find the start of a sequence** (pattern L)
- Tried: a map of sequence tops, extending a sequence when `val + 1` exists. Got stuck on whether a number fits at the start or the end of a sequence.
- Missing idea: `x` starts a sequence exactly when `x - 1` is not in the set. Only count up from starts, because every other walk is part of a longer one (dominated work).
- Why my approach steps did not help: the order of the array does not matter, so "what do I know about everything before `i`?" has no useful answer. See section 7 of `../C++/ApproachForDSA.md`.

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
- Fix: swap the slots themselves, `swap(list[i], list[j])`, or assign `list[idx] = last`. See section 2 of `../C++/Notes_vectors.md`.

**6. Forgot to store the index on insert** (pattern 3, K)
- Wrote: `insert` with `list.push_back(val);` but no `freq[val] = list.size() - 1;`.
- Wrong because: the map stayed empty, so duplicates were inserted and `remove` always returned false.
- Lesson for 3, 4 and 6: write the invariant ("every value in `list` is in the map, and `list[map[v]] == v`") and check it after each method.

---

## 523. Continuous Subarray Sum (M)

**1. Stored a count instead of the first index** (pattern 7, K)
- Wrote: `sumIdxMap[mod]++;` on every step, a line carried over from the 974 counting solution.
- Wrong because: the check `i - sumIdxMap[mod] >= 2` needs the index where `mod` first appeared. `++` turned the value into a count and changed it on every visit. `[5, 0, 0, 0]`, `k = 3` returned false, missing `[0, 0]`.
- Fix: `if (!modExist) sumIdxMap[mod] = i;`. Store the index, and only the first time, so the subarray found is as long as possible.

**2. Used `[]` to check if the key exists** (pattern 2, K)
- Wrote: `if (sumIdxMap[mod] && i - sumIdxMap[mod] >= 2)`.
- Wrong because: `[]` inserts a missing key with value 0, and a real index 0 reads as false, so a remainder first seen at index 0 looks missing.
- Fix: `bool modExist = sumIdxMap.find(mod) != sumIdxMap.end();`. See section 1 of `../C++/Notes_maps.md`.

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
- Fix: `int req = ((totalSumTillI % k) + k) % k;`. See section 1 of `../C++/Notes_math.md`.

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
