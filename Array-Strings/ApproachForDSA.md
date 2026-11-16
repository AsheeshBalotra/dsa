# Approach Notes (DSA interviews)

How to rebuild a solution from scratch instead of remembering its code. For each problem, remember one line: **what the brute force wastes, and what removes the waste**.

The steps, in order:
```
1. Constraints   how big is n, what time is allowed, sorted? negatives? duplicates?
2. Brute force   the simplest correct solution and its cost, inner loop looking back
3. Waste         what the inner loop repeats, and the tool that removes it
4. State         separate i from j, then what to remember about earlier elements
5. Dry run       run it by hand on a tiny input and the edge cases, then code
6. Better?       after it works: time at the lower bound? can an array become a variable?
```

If steps 3 and 4 give no clear answer, use step 7.

---

## 1. Read the constraints

The input size tells you how fast the solution must be. About 10⁸ simple steps run in one second.

| `n` up to | Allowed time       | Usually means      |
|-----------|--------------------|--------------------|
| 20        | O(2ⁿ)              | try every subset   |
| 500       | O(n³)              | three nested loops |
| 10⁴       | O(n²)              | two nested loops   |
| 10⁶       | O(n log n) or O(n) | sort, or one pass  |

Then check what the input looks like. Each answer opens or closes some tools:
- **Sorted:** binary search and two pointers become possible.
- **Negatives:** a sliding window on sums stops working (see the two rules in step 3).
- **Duplicates:** a `set` keeps only one copy, so count them with a map.
- **Return indexes:** do not sort, because sorting moves the indexes.

---

## 2. Write the brute force

**Brute force:** the simplest correct solution, usually trying every possibility with nested loops.
- Write it in one line with its cost, for example "for every pair (i, j), check the sum: O(n²)".
- In an interview, say it first. It shows you understand the problem, and it is your fallback answer.
- Step 3 works by improving it, so you need it in front of you.

**Write the inner loop looking back.** The outer loop `i` is the current element. The inner loop `j` scans the elements before it:
```cpp
for (int i = 0; i < n; i++)          // current element
    for (int j = 0; j < i; j++)      // looks back: everything before i
```

- The inner loop then scans exactly the elements already passed. So the state in step 4 is just a summary of what the inner loop scans, and the optimised code keeps the same outer loop and replaces only the inner loop.
- Each step adds one element, `nums[i]`, to "everything before", so the state is updated with that one element.
- Make the inner loop look forward (`j` from `i + 1` to `n - 1`) only when the question is about what comes after, like "next greater element" or "max to the right". For the one-pass version, walk `i` from right to left, so the elements after `i` are the ones already passed.

---

## 3. Find the waste

Ask: **"What is the inner loop doing?"** Most array and string problems waste work in one of these ways:

| The inner loop is...                                  | Fix                                         |
|-------------------------------------------------------|---------------------------------------------|
| searching for a partner value (`target - x`)          | hash map of values seen so far              |
| re-adding the same range of numbers                   | prefix sum                                  |
| comparing every pair to put items into groups         | hash map with a shared key                  |
| restarting a window from scratch                      | sliding window, if the two rules below hold |
| scanning forward for the next bigger or smaller value | monotonic stack                             |
| checking every value in a sorted range                | binary search                               |
| searching for a pair in a sorted array                | two pointers from both ends                 |
| sorting everything to get the top k                   | heap of size k (see `Notes_heap.md`)        |
| repeating work that a longer result already covers    | skip it with a condition (dominated work)   |

Terms:
- **Hash map** (`unordered_map`): stores key -> value pairs. Lookup and insert are O(1) on average.
- **Prefix sum:** `prefix[i]` is the total of elements 0 to `i`. The sum of a range `j..i` is `prefix[i] - prefix[j - 1]`.
- **Shared key:** a value that is the same for every item in a group, like the sorted letters of an anagram.
- **Sliding window:** two indexes, `left` and `right`, marking a range. Both only move forward.
- **Monotonic stack:** a stack kept in increasing (or decreasing) order. Items that break the order are popped.
- **Binary search:** on sorted data, check the middle and drop the half that cannot hold the answer.
- **Two pointers from both ends:** one index at the start and one at the end, moving toward each other.
- **Dominated work:** a computation whose result is always beaten by another one, so skipping it never loses the answer. How to find it: step 7.

**The two rules for a sliding window.** A window is **valid** when it meets the problem's condition (for example, no repeated letters). A sliding window works only if both rules hold. Otherwise use prefix sum + hash map.
- **Rule 1:** if a window is valid, every smaller window inside it is valid. So you can grow `right` without rechecking.
- **Rule 2:** if a window is invalid, every bigger window containing it is invalid. So once it breaks, move `left`.

Checking the rules on two conditions:
```
No repeated letters, "abcabcbb":
"abc" valid     -> "bc" valid                (Rule 1 holds)
"abca" invalid  -> "abcab" invalid           (Rule 2 holds)     -> window works

Sum <= k, negatives allowed, k = 2:
[3, -2] valid   -> [3] invalid               (Rule 1 breaks)
[3] invalid     -> [3, -2] valid             (Rule 2 breaks)    -> window fails
```

> LOOKUP_BY_ME — was: "Sum == k" examples (`[1,1,1]` valid and `[1,1]` invalid for k = 3). `== k` breaks the rules even with no negative numbers, so the example did not show what negatives do. Apply the rules to the condition the window keeps, such as `sum <= k`: with no negative numbers it holds both rules, and negatives break it.

> TODO: Expand — do the two rules hold for shortest-window problems such as 76 Minimum Window Substring? Work out how the window moves there.

---

## 4. Decide the state

**State:** what you remember about the elements already passed. Ask: **"At index `i`, what do I need to know about everything before `i`?"** Here `i` is the outer loop index (the current element), and "everything before `i`" is what the inner loop `j` from step 2 scans. The answer picks the data structure.

**Separate `i` from `j`.** Rewrite the inner loop's condition so that one side has only `j` and the other side has only `i`. The `i` side is fixed for this `i`, so the inner loop is just searching the earlier elements for one value.
- The `i` side tells you **what to search for**.
- What you need to know about it (does it exist, where, how many, the best one) picks the **data structure**.

Separating `i` from `j` in five problems:
```
1 Two Sum:   nums[j] + nums[i] == target      ->  nums[j] == target - nums[i]
121:         max of prices[i] - prices[j]     ->  min of prices[j]
560:         sum(j..i) == k                   ->  prefix[j-1] == prefix[i] - k
974:         sum(j..i) % k == 0               ->  prefix[j-1] % k == prefix[i] % k
1010:        (nums[j] + nums[i]) % k == 0     ->  nums[j] % k == (k - nums[i] % k) % k
```

- If `i` and `j` do not separate, as with `sum(j..i)`, transform first. A prefix sum turns a range into two separate values.
- With `% k`, a sum of two values searches for the opposite remainder and a difference searches for the same remainder. See "Which remainder to look for" in `Notes_math.md`.
- This works for problems about pairs `(j, i)`. Sliding window, two pointers, binary search and monotonic stack have their own key question (step 3).
- If the order of the array does not matter, only which values exist (as in 128), there is no useful "before `i`". See step 7.

In the table, `f(j)` is the `j` side, for example `nums[j]` or `prefix[j-1]`.

| You need to know...              | Use                            | Example                                  |
|----------------------------------|--------------------------------|------------------------------------------|
| does `f(j) == value` exist?      | `unordered_set`                | 217: `nums[j] == nums[i]`                |
| where is `f(j) == value`?        | `unordered_map` value -> index | 1 Two Sum: `nums[j] == target - nums[i]` |
| how many `f(j) == value`?        | `unordered_map` value -> count | 560: `prefix[j-1] == prefix[i] - k`      |
| the smallest or largest `f(j)`   | one variable                   | 121: min of `prices[j]`                  |
| how many `f(j) < value`          | Fenwick tree over the values   | 315 Count of Smaller Numbers After Self  |
| counts of letters `a` to `z`     | `int freq[26]`                 | 242 Valid Anagram                        |
| the most recent unmatched item   | `stack`                        | 20 Valid Parentheses: last open bracket  |
| the k largest or smallest so far | heap                           | 215 Kth Largest                          |

> LOOKUP_BY_ME — was: "sorted structure + binary search", example "`set` with `lower_bound`". `lower_bound` finds the position in O(log n), but counting the elements before it needs `distance`, which is O(n) on a `set`. A Fenwick tree adds a value and counts the values below `x` in O(log n) each.

> TODO: Expand — what a Fenwick tree stores and how it counts in O(log n).

---

## 5. Dry run, then code

**Dry run:** run the algorithm by hand on a small input, writing every variable after every step.
- Use 3 or 4 elements. A wrong value in your table means a wrong algorithm, and that is cheaper to fix now than in code.
- Then try the edge cases: empty input, one element, all values the same, negatives, overflow past `INT_MAX` (use `long long`), `k` larger than `n`.
- Write code only after the dry run works.

Dry run of 560, `nums = [1, 2, 3]`, `k = 3`:
```
i   nums[i]   prefix   prefix - k   seen before?   map after             count
-   -         0        -            -              {0:1}                 0
0   1         1        -2           no             {0:1, 1:1}            0
1   2         3        0            yes, 1 time    {0:1, 1:1, 3:1}       1     ([1,2])
2   3         6        3            yes, 1 time    {0:1, 1:1, 3:1, 6:1}  2     ([3])
```

---

## 6. Can it be better?

**When:** only after the code works. Go here if the time is above the lower bound (usually O(n), since every element must be read), or if you use an O(n) helper array and O(1) space is asked for ("can you do better?").

Ask in order:
1. **Is time at the lower bound?** No: back to step 3, there is more waste. Yes: only space can improve.
2. **Can each helper array become a variable?** Yes, if the main loop walks in the same direction the array is built.
3. **Does the answer need exact values, or only a comparison?** A `min` / `max` of two sides needs only the smaller or larger one, so the other can stay unknown: two pointers from both ends. A sum or product needs both exactly: keep one side in the output array (238).

### 42. Trapping Rain Water

```
Works:     water[i] = min(maxLeft, maxRight) - h[i]                     O(n) time, O(n) space
1. Time:   O(n) is the lower bound, so only space can improve
2. Arrays: maxLeft is built left to right like the loop -> a variable
           maxRight is built right to left -> still an array
3. Need:   only the smaller max. l and r walk in from both ends,
           leftMax = max(h[0..l]), rightMax = max(h[r..n-1])
           if leftMax < rightMax: l's real right max is >= rightMax > leftMax,
           so water[l] = leftMax - h[l]. Settle l, l++. Else settle r, r--   O(1) space
Remember:  only the smaller side matters -> two pointers, settle the smaller side
```

---

## 7. When the first approach does not work

**When to use this:** steps 3 and 4 give no clear answer. Two common reasons:
- The order of the array does not matter, only which values exist, so there is no useful "before `i`".
- No row of the waste table in step 3 fits the brute force.

**What to do: find the dominated work on paper** (dominated work is defined in step 3).
1. Make any cheap fix from the step 3 table first, for example a hash set instead of scanning for a value.
2. If the order does not matter, put every value in a set before the loop.
3. Dry-run the brute force on a small input and write down every result it computes, one row per start.
4. Ask: which rows are always worse than another row?
5. Ask: what condition tells me in advance that a row will lose? Skip those rows.

- 128 below is the clearest example. Kadane (53) and 121 also work by skipping dominated work: Kadane drops a negative running sum, and 121 keeps only the lowest price so far.
- **If nothing clicks in an interview:** give a simpler correct solution first, often by sorting in O(n log n), then do the dry-run table out loud. The interviewer sees how you reason, even if you need a hint.

### 128. Longest Consecutive Sequence

Brute force: try every number as a start, and rescan the array for the next number each time.
```cpp
for (int i = 0; i < n; i++) {                       // try every number as a start
    int next = nums[i] + 1, length = 1, j = 0;
    while (j < n) {                                 // scan the array for next
        if (nums[j] == next) { length++; next++; j = 0; continue; }   // found: rescan from 0
        j++;
    }
    ans = max(ans, length);
}
```
```
Brute:     for each start, rescan the array for next, next + 1, ...             O(n³)
Waste 1:   the inner loop searches for one known value (next) -> unordered_set  O(n²)
```

Dry run with the set, listing every walk, `nums = [4, 1, 3, 2]`:
```
start   walk          length
4       4             1
1       1, 2, 3, 4    4
3       3, 4          2
2       2, 3, 4       3
```

- The walk from 2 is part of the walk from 1, and the walk from 3 is part of the walk from 2.
- A walk inside a longer walk is always shorter, so it can never be the answer.

The steps that follow from it:
```
Waste 2:   walks that a longer walk already covers (dominated work)
           the walk from x is covered exactly when x - 1 is in the set
State:     unordered_set of all values, built before the loop
Algo:      for x in the set, skip if x - 1 exists, else count up x+1, x+2, ...  O(n)
           each number is walked over once in total; loop over the set, not nums,
           so duplicate starts do not repeat the same walk
Remember:  only count up from numbers whose x - 1 is missing
```

- **Fallback for 128:** sort and count runs of consecutive numbers, skipping duplicates: O(n log n).

---

## 8. How to practise

- **Comments first:** before coding, copy steps 1 to 5 from the top of this file as comments and fill them in. If you cannot fill in the waste, you are not ready to code.
- **Hints in levels:** if stuck after 25 minutes, look only at the pattern name and try again. Then look at the state. Read the code last.
- **Write the Remember line** in the checklist, in waste -> fix form, not as a summary of the code.
- **Re-solve cold** after 1 day, 1 week and 1 month, starting from the Remember line, not from your old code.
- **Rotate sections:** at most two problems in a row from one section, skipping solved ones (the order is in the checklist). The first builds the concept and the second tests it. Switching sections after that practises choosing the tool, which is the hard part in an interview. Before coding, say which pattern it is and why.
- **Say it aloud** while solving. Interviews need it, and it makes each step explicit.

**Practice:** 1 Two Sum, 49 Group Anagrams, 128 Longest Consecutive Sequence, 3 Longest Substring Without Repeating Characters, 1010 Pairs of Songs With Total Durations Divisible by 60, 560 Subarray Sum Equals K, 974 Subarray Sums Divisible by K, 42 Trapping Rain Water.

---

## 9. Worked examples

Each problem goes through the steps. The last line is the one to remember.

### 1. Two Sum

```
Brute:     for each i, loop j < i looking for nums[j] == target - nums[i]       O(n²)
Waste:     the inner loop searches for one known value
State:     map value -> index of values seen so far
Algo:      at i, look up target - nums[i], then add nums[i]                     O(n)
Remember:  inner loop searches for a partner -> hash map
```

### 49. Group Anagrams (`L` = word length)

```
Brute:     compare each word with each group using isAnagram (242)              O(n² · L)
Waste:     comparing pairs to form groups
Ask:       what is the same for every word in a group? its sorted letters
State:     map sorted word -> list of words
Algo:      "eat", "tea", "ate" all go under the key "aet"                       O(n · L log L)
Remember:  group by a shared key
```

### 3. Longest Substring Without Repeating Characters

```
Brute:     for each start, extend until a repeat                                O(n²)
Waste:     after a repeat, restarting at the next start rechecks letters already known unique
Check:     both window rules hold, so use a sliding window
State:     last index of each letter
Algo:      on a repeat, move left to one past the earlier copy (never backward) O(n)
Remember:  window rules hold -> grow right, jump left
```

### 560. Subarray Sum Equals K

```
Check:     negatives allowed, so the window rules fail
Brute:     for each i, loop j backward adding up sum(j..i)                      O(n²)
Waste:     re-adding ranges -> prefix sum, sum(j..i) = prefix[i] - prefix[j-1]
           the inner loop now searches for prefix[j-1] == prefix[i] - k
State:     map prefix sum -> times seen, starting with {0: 1} so subarrays from index 0 count
Algo:      at i, add map[prefix - k] to the count, then record prefix           O(n)
Remember:  Two Sum on prefix sums
```

**Divisible by `k`:** the next two use remainders as hash keys. For why each one looks for its remainder, see "Which remainder to look for" in `Notes_math.md`.

### 1010. Pairs of Songs With Total Durations Divisible by 60 (`k = 60`)

```
Brute:     for each i, loop j < i checking (time[j] + time[i]) % k == 0         O(n²)
Waste:     the inner loop searches for one known remainder
Mod:       r = time[i] % k. A sum: a + b ≡ 0 means b ≡ -r, so look for -r
Look up:   -r is negative, so look it up as -r + k, written (k - r) % k
           r = 0 looks for 0, since (k - 0) % k = 0, not k
State:     int count[60]: remainder -> times seen
Algo:      at i, add count[(k - r) % k], then count[r]++                        O(n)
Remember:  Two Sum on remainders: a sum looks for the opposite remainder
```

### 974. Subarray Sums Divisible by K

```
Check:     negatives allowed, so the window rules fail
Brute:     for each i, loop j backward checking sum(j..i) % k == 0              O(n²)
Waste:     re-adding ranges -> prefix sum, sum(j..i) = prefix[i] - prefix[j-1]
Mod:       r = prefix[i] % k. A difference: prefix[i] - prefix[j-1] ≡ 0 means the same
           remainder, so look for r
Store:     prefix can be negative: -7 % 5 is -2 in C++. Store it as -2 + 5 = 3,
           using ((prefix % k) + k) % k, so equal remainders match as keys
State:     vector<int> count(k): remainder -> times seen, starting with count[0] = 1
           so subarrays from index 0 count
Algo:      at i, add count[r], then count[r]++                                  O(n)
Remember:  560 on remainders: a difference looks for the same remainder
```
