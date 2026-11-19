# Techniques Notes (DSA interviews)

Named algorithms and techniques from the problems I solve. Related: `Checklist_String_Array.md` (the problems), `DSA_Approach_Framework.md` (how to work out a solution), `My_DSA_Mistake_Patterns.md` (the mistakes I repeat).

Each section, short and to the point: the name, the problems that use it, what it is, then one working example with a dry run. A very common technique can instead get only "when to think of it".

## 1. Kadane's algorithm

Used in:
- 53 Maximum Subarray: the problem itself
- 121 Best Time to Buy and Sell Stock: on the daily price changes
- 152 Maximum Product Subarray: track the max and the min, because a negative swaps them
- 918 Maximum Sum Circular Subarray: max(Kadane, total − minimum subarray), but Kadane alone when every number is negative, because then the formula gives 0, the empty subarray
- 1186 Maximum Subarray Sum with One Deletion: two states, with and without the deletion

**Kadane's algorithm:** finds the largest sum of a contiguous subarray in one pass, O(n) time and O(1) space. At each index, the best subarray ending there either extends the previous one or restarts here, whichever is bigger.

- **Why it works:** a negative running sum only makes later subarrays smaller, so dropping it never loses the answer.
- **Breaks when dropping a negative prefix is not safe:** products, wrap-around, length limits, an exact target sum (560 needs prefix sum + hash map).
- **Trap:** start `maxSum` from `nums[0]`, not 0. Otherwise an all-negative array such as `[-3, -1, -2]` returns 0 instead of -1.

Example, 53 Maximum Subarray:
```cpp
int currSum = 0, maxSum = nums[0];
for (int x : nums) {
    currSum = max(x, currSum + x);   // extend or restart, whichever is bigger
    maxSum = max(maxSum, currSum);
}
```

Dry run on `[-2, 1, -3, 4, -1, 2, 1, -5, 4]`:
```
x    max(x, currSum + x)    choice     currSum   maxSum
-2   max(-2, -2)            restart    -2        -2
1    max(1, -1)             restart    1         1
-3   max(-3, -2)            extend     -2        1
4    max(4, 2)              restart    4         4
-1   max(-1, 3)             extend     3         4
2    max(2, 5)              extend     5         5
1    max(1, 6)              extend     6         6       ([4, -1, 2, 1])
-5   max(-5, 1)             extend     1         6
4    max(4, 5)              extend     5         6
                                                 -> 6
```

---

## 2. Prefix sum + hash map

Used in:

| Problem                             | Key in the map                         | Value       | Seed      | Look up at `i`                 |
|-------------------------------------|----------------------------------------|-------------|-----------|--------------------------------|
| 560 Subarray Sum Equals K           | prefix                                 | count       | `{0: 1}`  | `prefix - k`                   |
| 974 Subarray Sums Divisible by K    | `prefix % k`                           | count       | `{0: 1}`  | the same remainder             |
| 523 Continuous Subarray Sum         | `prefix % k`                           | first index | `{0: -1}` | the same remainder, length ≥ 2 |
| 525 Contiguous Array                | prefix, with each 0 counted as -1      | first index | `{0: -1}` | the same prefix                |
| 1010 Pairs of Songs Divisible by 60 | `t % 60` (a pair of values, no prefix) | count       | none      | `(60 - r) % 60`                |

**Prefix sum + hash map:** `prefix[i]` is the sum of `nums[0..i]`, so `sum(j..i) = prefix[i] - prefix[j-1]`. The condition on a subarray becomes a condition on two prefixes. Walk `i` once, keep the earlier prefixes in a hash map, and look up the one prefix that makes `j..i` valid. O(n) time, O(n) space.

- **Count or longest:** to count subarrays, the map value is a count, seeded `{0: 1}`. For the longest subarray, it is the first index, seeded `{0: -1}` and written only the first time.
- **Divisible by k:** `sum(j..i) % k == 0` means `prefix[i]` and `prefix[j-1]` have the same remainder, so look up the same remainder. A sum of two values (1010) looks up the opposite one, `(k - r) % k`. See "Which remainder to look for" in `cpp-notes/Notes_math.md`.
- **Instead of a sliding window:** use it when negatives are allowed, because then the window rules fail.
- **Trap:** C++ `%` keeps the sign, so `-1 % 2` is -1, not 1. Store `((prefix % k) + k) % k`. On `[-1, 2, 9]`, k = 2, the plain `%` misses `[2]` and returns 1 instead of 2.

Example, 974 Subarray Sums Divisible by K:
```cpp
vector<int> count(k, 0);                         // remainder -> times seen
count[0] = 1;                                    // the empty prefix, so subarrays from index 0 count
int prefix = 0, ans = 0;
for (int x : nums) {
    prefix += x;
    int r = ((prefix % k) + k) % k;              // 0..k-1, even when prefix is negative
    ans += count[r];                             // earlier prefixes with the same remainder
    count[r]++;
}
```

Dry run on `[4, 5, 0, -2, -3, 1]`, k = 5:
```
x    prefix   r   count[r] before   ans   counts after (remainder: times)
                                      0     {0: 1}
4    4        4   0                 0     {0: 1, 4: 1}
5    9        4   1                 1     {0: 1, 4: 2}
0    9        4   2                 3     {0: 1, 4: 3}
-2   7        2   0                 3     {0: 1, 2: 1, 4: 3}
-3   4        4   3                 6     {0: 1, 2: 1, 4: 4}
1    5        0   1                 7     {0: 2, 2: 1, 4: 4}
                                    -> 7
```

---

## 3. Two pointers

Used in: 125, 167, 15, 11, 42 (from both ends); 26 (read and write). 75 uses three pointers: section 8.

When to think of it:
- **From both ends:** the array is sorted (or can be sorted, because no indexes are returned) and I look for a pair or triplet against a target, or the answer depends on the two ends (palindrome, container). Each step, move the pointer whose side cannot do better.
- **Read and write, same direction:** the array must be changed in place (remove, dedupe, merge). The write pointer marks the end of the part I keep, and both pointers only move forward.

---

## 4. Dynamic programming

Used in: 55 Jump Game (also has an O(n) greedy), 139 Word Break, 1143 Longest Common Subsequence. More in `../Checklist_Other_Topics.md`: 70, 198, 322.

When to think of it:
- The answer for a state is built from answers to smaller states of the same kind, and plain recursion computes the same states again and again: "number of ways", "min or max cost", "can I reach or build it".
- **Greedy first:** if one running value covers every state, a greedy is enough. In 55, the farthest reachable index replaces the whole `dp` array: O(n) time, O(1) space.
- **Top-down:** recursion + memoization, starts from the big question.
- **Bottom-up:** iteration + a table, starts from the base cases and loops in the direction where each state's inputs are already filled.
- **Cost:** time = states × choices per state × cost per choice. Space = the table, plus the call stack for top-down. In 55: n states × up to n jumps × O(1) = O(n²). In 139: n states × n cuts × O(n) per cut (`substr` plus a hash) = O(n³), or O(n · L²) when the cuts stop at the longest word, length L.
- **Without a memo:** count the paths that reach one state. In 139 the number of calls multiplies with every extra letter, so plain recursion is exponential.

Full notes: sections 10 (recursion -> memoization -> iterative, top-down vs bottom-up, space optimisation, common bugs) and 11 (recursion direction) of `cpp-notes/CplusNotes.md`.

---

## 5. Fenwick tree

Used in: 315 Count of Smaller Numbers After Self (the "how many `f(j) < value`" row in step 4 of `DSA_Approach_Framework.md`). 307 Range Sum Query - Mutable is the plain version.

**Fenwick tree (binary indexed tree):** an array where `bit[i]` holds the total of a range of positions ending at `i`. The range is as long as the lowest set bit of `i`, which is `i & -i` (section 4 of `cpp-notes/Notes_bits.md`). It adds to one position and sums a prefix, both in O(log n).

What each `bit[i]` covers, for n = 8:
```
i    binary   i & -i   bit[i] holds the total of positions
1    0001     1        1
2    0010     2        1..2
3    0011     1        3
4    0100     4        1..4
5    0101     1        5
6    0110     2        5..6
7    0111     1        7
8    1000     8        1..8
```

- **Sum of positions 1..i:** add `bit[i]`, then remove the lowest set bit of `i`, and repeat until 0. `sum(7)` reads `bit[7] + bit[6] + bit[4]`, the ranges 7, 5..6 and 1..4.
- **Add to position i:** add to `bit[i]`, then add the lowest set bit to `i`, and repeat while `i <= n`. `add(3)` updates `bit[3]`, `bit[4]` and `bit[8]`, the ranges that contain 3.
- **Why O(log n):** each step of `sum` clears one bit of `i`, and each step of `add` moves `i` up to a bigger power-of-two boundary. Either way there are at most log n steps.
- **Counting values below `x`:** use the values as positions, and add 1 at a value's position when you see it. Then `sum(x - 1)` counts the values smaller than `x` seen so far.
- **Trap:** the tree is 1-indexed. `add(0, ...)` loops forever, because `0 & -0` is 0, so `i` never moves.

Example, 315 Count of Smaller Numbers After Self:
```cpp
struct Fenwick {
    vector<int> bit;                                       // 1-indexed: bit[0] is unused
    Fenwick(int n) : bit(n + 1, 0) {}
    void add(int i, int delta) {                           // count at position i += delta
        for (; i < (int)bit.size(); i += i & -i) bit[i] += delta;
    }
    int sum(int i) {                                       // total of positions 1..i
        int s = 0;
        for (; i > 0; i -= i & -i) s += bit[i];
        return s;
    }
};

vector<int> countSmaller(vector<int>& nums) {
    vector<int> sorted = nums;                             // rank the values 1..m, so
    sort(sorted.begin(), sorted.end());                    // negative and huge values fit
    sorted.erase(unique(sorted.begin(), sorted.end()), sorted.end());
    Fenwick fw(sorted.size());
    vector<int> ans(nums.size());
    for (int i = nums.size() - 1; i >= 0; i--) {           // right to left: the tree holds what is after i
        int r = lower_bound(sorted.begin(), sorted.end(), nums[i]) - sorted.begin() + 1;
        ans[i] = fw.sum(r - 1);                            // values smaller than nums[i]
        fw.add(r, 1);
    }
    return ans;
}
```

O(n log n) time, O(n) space.

Dry run on `[5, 2, 6, 1]`, ranks 1 -> 1, 2 -> 2, 5 -> 3, 6 -> 4:
```
i   nums[i]   rank   sum(rank - 1)   ans[i]   ranks seen after
3   1         1      sum(0) = 0      0        {1}
2   6         4      sum(3) = 1      1        {1, 4}
1   2         2      sum(1) = 1      1        {1, 2, 4}
0   5         3      sum(2) = 2      2        {1, 2, 3, 4}
                                     -> [2, 1, 1, 0]
```

---

## 6. Sliding window

Used in: 3 Longest Substring Without Repeating Characters, 424 Longest Repeating Character Replacement, 76 Minimum Window Substring (the flipped rules). The same "budget k" disguise as 424: 1004 Max Consecutive Ones III (the simplest form), 1208 Get Equal Substrings Within Budget, 2024 Maximize the Confusion of an Exam, 1838 Frequency of the Most Frequent Element.

**Sliding window:** `left` and `right` mark a range, and both only move forward. In the variable-size form, grow `right` one step at a time, and move `left` while the window breaks the condition. Each index enters and leaves the window once, so it is O(n) instead of the O(n²) of trying every range.

Three types, easiest to spot first:
1. **Fixed size:** the length k is given ("every window of size k"). Add the new element, remove the one k back. 643 Maximum Average Subarray I, 567 Permutation in String.
2. **Sum, positive numbers only:** keep one `int sum`. For the shortest with sum >= k (209), grow `right`, then move `left` while the sum stays >= k. Negatives break it: use section 2.
3. **A condition on what the window holds, often a budget k (hardest to spot, the one to remember):** "at most k replacements / flips", "no repeats", "contains all of `t`". Turn it into a test on one window, then pick the state the test reads: the four questions and the state table below. 3, 424, 76, 1004, 904.

> [!IMPORTANT]
> Focus on type 3: a variable-size window that needs a data structure (`count[26]`, a map, a deque) to keep its state. It is the hardest to spot, and the type I could not solve in 424. Before coding, write the state as "reads / enters / leaves" (see "State: what to keep, and how it changes" below).

When to think of it, four questions in order:
1. **Is the answer one contiguous piece?** "Longest / shortest / number of substrings or subarrays".
2. **Can I write the condition as a test on one window, with no start or target fixed in advance?** When the problem describes operations ("replace / flip / delete at most k"), the k is a budget: write what the operations cost for one window, and the test is cost <= k. In 424, every letter except the most frequent one must be replaced, so the test is `length - maxFreq <= k`.
3. **Can the test be updated in O(1) when one element enters or leaves?** Keep counts (`count[26]`), a sum, or the number of distinct values.
4. **Do the two window rules hold?** See "The two rules for a sliding window" in step 3 of `DSA_Approach_Framework.md`, and the flipped rules for a shortest window. If they break, as with sums and negative numbers, use prefix sum + hash map (section 2).

- **My brute force is the clue:** "for each start `i`, extend right while it can still be valid" scans almost the same range again when `i` moves to `i + 1`. If question 2 has an answer, the window removes that rescan. In 424 I wrote exactly this brute force and did not see the window.
- **The trap in question 2:** fixing the target in advance (in 424, "make everything equal to `s[i]`") ties the window to its start. The test must let each window pick its own best choice, such as the most frequent letter.

Shape of the loop, for "longest":
```
for each right:
    add s[right] to the window's counts
    while the window is invalid:
        remove s[left] from the counts, left++
    best = max(best, right - left + 1)
```

### State: what to keep, and how it changes

**Window state:** the values the validity test reads, kept up to date as the window moves, so the test costs O(1) instead of rescanning the window. Sometimes it is one variable, sometimes a data structure.

How to find it:
1. Write the test first (question 2), in terms of the window.
2. List what the test reads. That list is the state. Pick the smallest thing that holds it: an `int` when one number is enough, a structure only when the test needs more.
3. For each piece of state, write how it changes when an element **enters** (at `right`) and when one **leaves** (at `left`). If both are O(1), the window is O(n).

What the test reads, and the state for it:
```
the test reads             state                        enters                leaves                        example
a sum                      int sum                      sum += x              sum -= x                      209
how many of one kind       int counter                  +1 if that kind       -1 if that kind               1004 (zeros)
how many of each value     count[26] / unordered_map    count[x]++            count[x]--                    3, 424, 76
how many distinct values   count map + int distinct     distinct++ at 0 -> 1  distinct-- at 1 -> 0          904
the max or min             26 counts, monotonic deque,  compare with x        the max may have left: hard   424, 1438
                           or multiset
```

- **Leaving is the hard direction for a max or min.** When the largest element leaves, the new max is unknown without looking. With few possible values (26 letters), rescan the counts: O(26). Otherwise use a monotonic deque (1438, or 239 for a fixed size) or a `multiset` (O(log n) per step).
- **The test decides the structure.** In 1004 the test is "zeros <= k", so one counter is enough. Picking a structure first can mean a map where one `int` would do.

### Worked example: 424 Longest Repeating Character Replacement

```
Test:      window length - maxFreq <= k      (replace every letter except the most frequent one)
Reads:     the count of the most frequent letter in the window
State:     count[26], letter -> times in the window. maxFreq comes from these counts
Enters:    count[s[right]]++. maxFreq can only go up, and only to this count
Leaves:    count[s[left]]--. maxFreq may go down: rescan the 26 counts
```

```cpp
int characterReplacement(string s, int k) {
    vector<int> count(26, 0);                                  // letter -> times in the window
    int left = 0, best = 0;
    for (int right = 0; right < s.size(); right++) {
        count[s[right] - 'A']++;                               // s[right] enters
        int maxFreq = *max_element(count.begin(), count.end());
        while ((right - left + 1) - maxFreq > k) {             // more than k letters to replace
            count[s[left] - 'A']--;                            // s[left] leaves
            left++;
            maxFreq = *max_element(count.begin(), count.end());   // the max may have left: rescan 26
        }
        best = max(best, right - left + 1);
    }
    return best;
}
```

O(26 · n) time, which is O(n), and O(1) space (26 counts).

Dry run on `"AABABBA"`, k = 1:
```
right  enters  counts   window     len - maxFreq   action                        best
0      A       A1       "A"        1 - 1 = 0       valid                         1
1      A       A2       "AA"       2 - 2 = 0       valid                         2
2      B       A2 B1    "AAB"      3 - 2 = 1       valid (replace the B)         3
3      A       A3 B1    "AABA"     4 - 3 = 1       valid                         4
4      B       A3 B2    "AABAB"    5 - 3 = 2       invalid: A leaves, left = 1
               A2 B2    "ABAB"     4 - 2 = 2       invalid: A leaves, left = 2
               A1 B2    "BAB"      3 - 2 = 1       valid                         4
5      B       A1 B3    "BABB"     4 - 3 = 1       valid                         4
6      A       A2 B3    "BABBA"    5 - 3 = 2       invalid: B leaves, left = 3
               A2 B2    "ABBA"     4 - 2 = 2       invalid: A leaves, left = 4
               A1 B2    "BBA"      3 - 2 = 1       valid                         4
                                                                                 -> 4
```

- No target letter is fixed: each window uses its own most frequent letter. On `"BAAA"`, k = 1, the window `"BAAA"` needs 4 - 3 = 1 replacement, so the answer is 4, the case my per-start approach missed.

The common O(n) version never lowers `maxFreq`, and moves the window by one instead of shrinking it:
```cpp
maxFreq = max(maxFreq, ++count[s[right] - 'A']);       // only the entering letter's count went up
if ((right - left + 1) - maxFreq > k) {                // shift the window by one, never shrink it
    count[s[left] - 'A']--;
    left++;
}
best = max(best, right - left + 1);
```

- **Why a stale `maxFreq` is safe:** the answer only grows when some window has a bigger `maxFreq` than any before. A stale, too-high `maxFreq` can only keep the window at a size already reached, never make it bigger, so `best` is never wrong.
- In an interview, the rescan version is enough and easier to justify. Mention this one as the follow-up.

Both versions return 4 on `"AABABBA"` (k = 1), `"ABAB"` (k = 2) and `"BAAA"` (k = 1), and matched a brute force on 20,000 random strings.

---

## 7. Boyer-Moore voting

Used in: 169 Majority Element (the problem itself), 229 Majority Element II (every value that appears more than n/3 times: two candidates and two counts).

**Boyer-Moore voting:** finds the value that appears more than n/2 times in one pass, O(n) time and O(1) space. Keep a candidate and a count: the same value adds 1, a different value subtracts 1, and when the count is 0 the next value becomes the candidate.

- **Why it works:** each decrement cancels one pair of different values. The majority appears more than n/2 times, so the other values cannot cancel all its copies, and it is the one left.
- **Trap:** the result is only a candidate. If the problem does not promise a majority, count the candidate in a second pass.

Example, 169 Majority Element:
```cpp
int candidate = nums[0], count = 0;
for (int x : nums) {
    if (count == 0) candidate = x;          // the old candidate was cancelled out
    count += (x == candidate) ? 1 : -1;
}
return candidate;
```

Dry run on `[2, 2, 1, 1, 1, 2, 2]`:
```
x   candidate   count
2   2           1
2   2           2
1   2           1
1   2           0
1   1           1       (the count was 0, so 1 becomes the candidate)
2   1           0
2   2           1       (the count was 0, so 2 becomes the candidate)
                -> 2
```

---

## 8. Dutch national flag (three-way partition)

Used in: 75 Sort Colors (the problem itself), 2161 Partition Array According to Given Pivot, and quicksort with many duplicates (split into < pivot, == pivot and > pivot). 283 Move Zeroes and 905 Sort Array By Parity are the two-region version.

**Dutch national flag:** sorts an array of three values in one pass, O(n) time and O(1) space, by keeping four regions. Each step checks `nums[mid]`, the first unchecked value, and moves it into its region, so the unchecked region shrinks by one every step.

The four regions:
```
[0, low)       all 0s
[low, mid)     all 1s
[mid, high]    not checked yet
(high, n-1]    all 2s
```

- **The asymmetry that makes it work:** after a swap with `high`, `mid` stays, because the value that came from `high` is unchecked. After a swap with `low`, `mid` moves on, because the value that came from `low` is a 1 already checked (or `low == mid`).
- **Trap:** loop while `mid <= high`, not to the end: everything after `high` is already a 2.

Example, 75 Sort Colors:
```cpp
int low = 0, mid = 0, high = nums.size() - 1;
while (mid <= high) {
    if (nums[mid] == 0) { swap(nums[low], nums[mid]); low++; mid++; }
    else if (nums[mid] == 2) { swap(nums[mid], nums[high]); high--; }   // mid stays: the new nums[mid] is unchecked
    else mid++;
}
```

Dry run on `[2, 0, 2, 1, 1, 0]`:
```
nums                 low   mid   high   nums[mid]   action
[2, 0, 2, 1, 1, 0]   0     0     5      2           swap with high, high--
[0, 0, 2, 1, 1, 2]   0     0     4      0           swap with low, low++, mid++
[0, 0, 2, 1, 1, 2]   1     1     4      0           swap with low, low++, mid++
[0, 0, 2, 1, 1, 2]   2     2     4      2           swap with high, high--
[0, 0, 1, 1, 2, 2]   2     2     3      1           mid++
[0, 0, 1, 1, 2, 2]   2     3     3      1           mid++
                     2     4     3                  mid > high: done
-> [0, 0, 1, 1, 2, 2]
```

---

## 9. XOR cancellation

Used in: 136 Single Number (the problem itself), 268 Missing Number (XOR every index and every value), 389 Find the Difference (the same on characters), 260 Single Number III (two unpaired values: split the array by a bit where they differ). 137 Single Number II is the exception: every other value appears three times, so count each bit modulo 3 instead.

**XOR cancellation:** `a ^ a = 0` and `a ^ 0 = a`, and the order does not matter. So XOR-ing every value cancels each pair and leaves the value with no partner. O(n) time, O(1) space.

- **Why it works:** XOR works bit by bit. A bit that appears an even number of times becomes 0, so only the bits of the unpaired value survive.
- The code for 136 and 268 is in section 5 of `cpp-notes/Notes_bits.md`.

Dry run of 136 on `[4, 1, 2, 1, 2]`, `result ^= x` for each `x`:
```
x   result (binary)   result
4   100               4
1   101               5
2   111               7
1   110               6       (the second 1 cancels the first)
2   100               4       (the second 2 cancels the first)
                      -> 4
```
