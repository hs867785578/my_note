# Sliding Window

Sliding window can also be called `two pointers`, it use two pointers to define a sub-array of the origin array and keeps the `loop invariant` while steping on. The main idea behind the sliding window technique is to convert two nested loops into a single loop. And if the problem is to check something in a sub-array, sliding windwo may be applied.

The process of sliding window is:

1. define the range $[left, right)$ with `left` and `right` pointers;
2. determine to iterate `left` or `right` boundary and extend the other boundary;
3. keep the `invariant` in loop and collect the results.

## Fixed Size Window

Sometimes we have to find the minimum/maximum/average of a fixed-size window, here comes the technique that move left and right pointer equally. The general code is:

```C++
int m = arr.size(), l = 0, r = 0, sum = 0;
while (r < m) {
  while (r < m && r - l < size) sum += arr[r++];
  // collect answers
  sum -= arr[l++];
}

```

- [Maximum Average Subarray I](https://leetcode-cn.com/problems/maximum-average-subarray-i/)
- [Grumpy Bookstore Owner](https://leetcode-cn.com/problems/grumpy-bookstore-owner/)
- [Maximum Points You Can Obtain From Cards](https://leetcode-cn.com/problems/maximum-points-you-can-obtain-from-cards/)
- [Maximum Number of Vowels in a Substring of Given Length](https://leetcode-cn.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)

## Flexible Size Window

In some situations the window size is not fixed, for example, we want to find a window whose sum is less or equal a target, the number of elements is not fixed, that's where the flexible-size window works.

The pattern is like:

```C++
int m = arr.size(), l = 0; r = 0;
while (r < m) {
  window.add(arr[r++]);
  while (is_invalid(window)) window.remove(arr[l++]);
  // Collect the answers here
}
```

- [Minimum Operations to Reduce X to Zero](https://leetcode-cn.com/problems/minimum-operations-to-reduce-x-to-zero/)
- [Minimum Window Substring](https://leetcode-cn.com/problems/minimum-window-substring/)
- [Longest Repeating Character Replacement](https://leetcode-cn.com/problems/longest-repeating-character-replacement/)
- [Minimum Number of K consecutive bit flips](https://leetcode-cn.com/problems/minimum-number-of-k-consecutive-bit-flips/)
- [Minimum Window Subsequence](https://leetcode-cn.com/problems/minimum-window-subsequence/)
- [Find K Length Substrings with No Repeated Characters](https://leetcode-cn.com/problems/find-k-length-substrings-with-no-repeated-characters/)
- [Minimum Swaps to Group All 1s Together](https://leetcode-cn.com/problems/minimum-swaps-to-group-all-1s-together/)
- [Diet Plan Performance](https://leetcode-cn.com/problems/diet-plan-performance/)

## Count Prolems in Window

Some problems also need the times of number or charactor's appearance, we can use an array or hash to store them.

And if we want to count the number of elements in a range `[low, high]`, we can convert the problem to find the required elements less or equal to a number, and then return `less_or_equal(high) - less_or_equal(low - 1)`.

- [Longest Substring with at most Two Distinct Characters](https://leetcode-cn.com/problems/longest-substring-with-at-most-two-distinct-characters/)
- [Number of Subarrays with Bounded Maximum](https://leetcode-cn.com/problems/number-of-subarrays-with-bounded-maximum/)
- [Subarrays with k Different Integers](https://leetcode-cn.com/problems/subarrays-with-k-different-integers/)

## Other Properties in Window

This class of problems needs not only sliding window, but also other data structures to get the required features effeciently.

- [Sliding Window and Two Pointers](https://leetcode-cn.com/leetbook/read/sliding-window-and-two-pointers/rurimj/)
- [Sliding Window Median](https://leetcode-cn.com/problems/sliding-window-median/)
- [Contains Duplicate III](https://leetcode-cn.com/problems/contains-duplicate-iii/)
- [K Empty Slots](https://leetcode-cn.com/problems/k-empty-slots/)
- [Longest Continuous Subarray with Absolute Diff Less Than or Equal to Limit](https://leetcode-cn.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)
