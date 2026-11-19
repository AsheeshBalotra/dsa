# DSA Checklist: Strings and Arrays/Vectors (SDE 2)

One problem per idea. A problem is **core** if it is widely asked (in two or more of the big curated interview lists, or the best one for learning its pattern) and it teaches something no other core problem teaches. Popular problems that repeat a core idea are under [Extras](#extras), for mixed practice once the core is done.

**List tags:** `B` = Blind 75, `G` = Grind 75, `N` = NeetCode 150, `T` = LeetCode Top Interview 150. No tag = classic pattern problem that is not in those lists.
**★** = in a "top 25 interview questions" list. Its other 13 are in [Checklist_Other_Topics.md](../Checklist_Other_Topics.md).
**Difficulty:** E / M / H. **Premium** problems have a free copy on LintCode (number given).

**Progress:** 35 of 41 core solved. Also solved: 6 extras, 3 warm-ups.

**Order:** at most two problems in a row from one section, solved ones skipped. The first builds the concept, the second tests it. Before coding, say which pattern it is and why.
```
Round 1:  121, 3 → 20, 155 → 34, 33 → 56, 435 → 53, 55 → 54, 73 → 8 → 136 → 169 → 5, 139 → 75
Round 2:  424, 76 → 739, 84 → 875 → 253 → 1143
```

---

## Concepts revised

- [x] char arithmetic, ASCII values, `c - 'a'`, `c - '0'` (Notes_strings.md)
- [x] `stoi`, `to_string`, overflow and range (Notes_strings.md)
- [x] `substr`, `find` / `rfind`, `erase`, `reverse` (Notes_strings.md)
- [x] `tolower` / `toupper`, `isupper` / `islower` (Notes_strings.md)
- [x] Number bases: base 2, 7, 16, 64 (Notes_strings.md, Notes_bits.md)
- [x] Bit operators, precedence, two's complement (Notes_bits.md)
- [x] `vector` basics: `push_back`, `resize`, 2D vectors, `sort` with custom comparator
- [x] `unordered_map` / `unordered_set` / `map` basics
- [ ] `stack`, `queue`, `deque`, `priority_queue` basics

---

## 1. Hashing and frequency
Use when: "have I seen this before?", grouping, counting in O(1).

- [x] 1. Two Sum (E) [B G N T] ★: map value to index
- [x] 242. Valid Anagram (E) [B G N T]: `freq[26]`
- [x] 49. Group Anagrams (M) [B N T]: sorted string or count as the key. [Couldn't solve]
- [x] 347. Top K Frequent Elements (M) [B N]: count, then heap or bucket sort [Couldn't solve]
- [x] 128. Longest Consecutive Sequence (M) [B N T] ★: set, start only at sequence starts, loop over the set not `nums` [Couldn't solve]
- [x] 380. Insert Delete GetRandom O(1) (M) [T]: vector + index map, swap with last

## 2. Prefix sum
Use when: range sums, products of "everything except me", "subarray sum equals k".

- [x] 238. Product of Array Except Self (M) [B G N T] ★: prefix and suffix products
- [x] 560. Subarray Sum Equals K (M): prefix sum + count map (very frequently asked)
- [x] 523. Continuous Subarray Sum (M): same remainder, store the first index, length >= 2

## 3. Two pointers
Use when: sorted array, pairs or triplets, comparing from both ends, or rewriting an array in place.

- [x] 26. Remove Duplicates from Sorted Array (E) [T]: read and write pointers, keep `nums[i]` only when it differs from the last kept value [Needs revision]
- [x] 75. Sort Colors (M) [G]: three pointers in one pass, 0s swapped to the front and 2s to the back (Dutch national flag)
- [x] 15. 3Sum (M) [B G N T] ★: sort, fix one, two pointers, skip duplicates
- [x] 11. Container With Most Water (M) [B G N T] ★: move the shorter side
- [x] 42. Trapping Rain Water (H) [G N T] ★: water = min(maxL, maxR) - h[i], only the smaller max matters: two pointers, settle the smaller side

## 4. Sliding window
Use when: "longest / shortest / count of subarray or substring with a condition".
Fixed size: add the new element, remove the old one. Variable size: grow right, shrink left while invalid.

- [x] 121. Best Time to Buy and Sell Stock (E) [B G N T]: track the minimum so far
- [x] 3. Longest Substring Without Repeating Characters (M) [B G N T] ★: last-seen index
- [x] 424. Longest Repeating Character Replacement (M) [B N] ★: window size - max freq <= k [Needs revision]
- [ ] 76. Minimum Window Substring (H) [B G N T] ★: need and have counts

## 5. Stack and monotonic stack
Use when: matching brackets, evaluating expressions, "next greater / smaller element".

- [x] 20. Valid Parentheses (E) [B G N T]: stack of open brackets
- [x] 155. Min Stack (M) [G N T]: store the minimum with each value
- [ ] 739. Daily Temperatures (M) [N]: stack of indexes, monotonic decreasing
- [ ] 84. Largest Rectangle in Histogram (H) [G N]: previous and next smaller

## 6. Binary search
Use when: sorted data, or "find the minimum value that works" (search on the answer).

- [x] 34. Find First and Last Position of Element in Sorted Array (M) [T]: lower and upper bound, `mid = lo + (hi - lo) / 2`
- [x] 33. Search in Rotated Sorted Array (M) [B G N T] ★: find the sorted half [Needs revision]
- [ ] 875. Koko Eating Bananas (M) [N] ★: binary search on the answer

## 7. Merge intervals
Use when: ranges, overlaps, meetings, scheduling. Usually sort by start first.

- [x] 56. Merge Intervals (M) [B G N T]: sort, extend or start new
- [x] 435. Non-overlapping Intervals (M) [B N]: sort by end, greedy
- [ ] 253. Meeting Rooms II (M) [B N], premium, free as LintCode 919: sort starts and ends, or min-heap

## 8. Kadane and greedy on arrays
Use when: best sum or reach over a contiguous range, local choice gives the global answer.

- [x] 169. Majority Element (E) [G T]: keep a candidate and a count, +1 for the same value, -1 for a different one, new candidate when the count hits 0
- [x] 53. Maximum Subarray (M) [B G N T]: Kadane, restart when sum < 0
- [x] 55. Jump Game (M) [B N T]: track the farthest reachable index [Needs revision]

## 9. Matrix and in-place arrays
Use when: 2D grids, or O(1) extra space is required.

- [x] 54. Spiral Matrix (M) [B G N T]: four boundaries [Needs revision]
- [x] 73. Set Matrix Zeroes (M) [B N T]: use first row and column as markers [Needs revision]

## 10. String manipulation and conversion
Use when: parsing, digits stored as strings, overflow.

- [x] 7. Reverse Integer (M) [N]: pop digits, check overflow before `* 10`
- [x] 151. Reverse Words in a String (M) [T]: reverse all, then each word
- [x] 8. String to Integer (atoi) (M) [G]: sign, leading zeros, clamp on overflow (the edge-case drill)

## 11. Palindromes and string DP
Use when: best substring or subsequence, or "can this string be built / matched".

- [x] 5. Longest Palindromic Substring (M) [B G N T]: expand around each center
- [x] 139. Word Break (M) [B G N T]: `dp[i]` = can split `s[0..i)` [Needs revision]
- [ ] 1143. Longest Common Subsequence (M) [B N]: 2D DP

## 12. Bits (see Notes_bits.md)

- [x] 231. Power of Two (E): `n > 0 && (n & (n - 1)) == 0`
- [x] 136. Single Number (E) [N T]: XOR everything

---

## Extras

Do these only after the core, picked in random order, to practise choosing the tool. Each one repeats a core idea (named), or takes a lot of time for how rarely it is asked.

- [x] 383. Ransom Note (E) [G T]: 242 again, `freq[26]`, decrement and check negative
- [x] 1010. Pairs of Songs With Total Durations Divisible by 60 (M): 1 on remainders, look up `(60 - t % 60) % 60`
- [x] 974. Subarray Sums Divisible by K (M): 560 with the remainder as the key, fix negative `%`
- [x] 125. Valid Palindrome (E) [B G N T]: two ends moving in, `isalnum`, `tolower`
- [x] 167. Two Sum II - Input Array Is Sorted (M) [N T]: the inner loop of 15
- [ ] 567. Permutation in String (M) [N]: fixed window with `freq[26]`, the easy case of 3 and 424 (438 is the same problem)
- [ ] 239. Sliding Window Maximum (H) [N]: monotonic deque, the window version of 739
- [ ] 150. Evaluate Reverse Polish Notation (M) [G N T]: the stack from 20, push numbers, pop two on an operator
- [ ] 704. Binary Search (E) [G N]: the template inside 34
- [ ] 153. Find Minimum in Rotated Sorted Array (M) [B N T] ★: same idea as 33, compare mid with right to find the sorted half
- [ ] 4. Median of Two Sorted Arrays (H) [N T]: partition both arrays (hard, rarely worth the time)
- [ ] 57. Insert Interval (M) [B G N T]: 56 without the sort, before / overlapping / after
- [ ] 152. Maximum Product Subarray (M) [B N]: 53, but track max and min because a negative swaps them
- [ ] 48. Rotate Image (M) [B N T]: transpose, then reverse rows (a trick to remember, not a pattern)
- [ ] 36. Valid Sudoku (M) [N T]: sets for rows, columns, boxes, box = `(r / 3) * 3 + c / 3`
- [ ] 189. Rotate Array (M) [T]: three reverses, the trick from 151
- [ ] 287. Find the Duplicate Number (M) [N]: Floyd cycle detection, do it after 141 Linked List Cycle
- [x] 13. Roman to Integer (E) [T]: subtract if smaller than next
- [ ] 14. Longest Common Prefix (E) [T]: compare character by character
- [ ] 271. Encode and Decode Strings (M) [B N], premium, free as LintCode 659: length prefix like `4#abcd`, read in parts as in 8
- [ ] 43. Multiply Strings (M) [N]: digit products into a result array, like 415 Add Strings
- [ ] 647. Palindromic Substrings (M) [B N]: 5, but count every center
- [ ] 72. Edit Distance (M) [N T]: the 2D table of 1143 with three choices, insert / delete / replace
- [ ] 191. Number of 1 Bits (E) [B N T]: the `n & (n - 1)` trick from 231, in a loop
- [ ] 268. Missing Number (E) [B N]: the XOR idea from 136, XOR of indexes and values
- [ ] 338. Counting Bits (E) [B N]: `dp[i] = dp[i >> 1] + (i & 1)`
- [ ] 190. Reverse Bits (E) [B N T]: shift out right, shift in left
- [ ] 371. Sum of Two Integers (M) [B N]: XOR for sum, AND + shift for carry
- [ ] 50. Pow(x, n) (M) [N T]: fast power, `long long` for `INT_MIN`

## Also solved (warm-ups, not core)
- [x] 344. Reverse String (E): two pointers swap
- [x] 387. First Unique Character in a String (E): count, then second pass
- [x] 415. Add Strings (E): one loop with `i >= 0 || j >= 0 || carry`

---

## SDE 2 interview tips
- Say the brute force first, then the optimised approach, with time and space for both.
- Mediums are the core of SDE 2 rounds. Aim to finish one in about 20 to 25 minutes.
- Check edge cases out loud: empty input, one element, all same, negatives, overflow (`INT_MIN`, `INT_MAX`).
- For each pattern above, be able to say in one line when to use it.
