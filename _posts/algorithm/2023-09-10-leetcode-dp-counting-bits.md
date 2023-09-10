---
title: '[Leetcode] 338. Counting Bits - JavaScript'
last_modified_at: 2023-09-10T17:20
categories:
  - Algorithm
tags:
  - leetcode
  - algorithm
  - dynamicprogramming
  - dp
toc: true
---

This algorithm problem is from [leetcode](https://leetcode.com/problems/counting-bits/description/).

## Problem 
Given an integer n, return an array ans of length n + 1 such that for each i (0 <= i <= n), ans[i] is the number of 1's in the binary representation of i.



Example 1:

Input: n = 2
Output: [0,1,1]
Explanation:
0 --> 0
1 --> 1
2 --> 10
Example 2:

Input: n = 5
Output: [0,1,1,2,1,2]
Explanation:
0 --> 0
1 --> 1
2 --> 10
3 --> 11
4 --> 100
5 --> 101


Constraints:

0 <= n <= 105


Follow up:

It is very easy to come up with a solution with a runtime of O(n log n). 
Can you do it in linear time O(n) and possibly in a single pass?
Can you do it without using any built-in function (i.e., like __builtin_popcount in C++)?


### Submission 1 - Shifting Bits
Runtime 81 ms  | Memory 49.4 MB \
I used bitwise `>>` operator and `&`.

Instead of counting number of 1's in given `i` one by one, I could use number of 1's in previously calculated number that is less than `i`
and has the common 1's with given `i`.
This picked number is `i/2`, which can be calculated by ` i >> 1`.
Since `i/2` or `i >> 1` will have the same number of 1's except the last bit, 
I add the last bit (1 or 0) of `i` to the number of 1's of `i/2` by doing `i & 1`.

```javascript
var countBits = function(n) {
    const outputArr = [];
    outputArr[0] = 0; 

    for (let i =1; i <= n; i++) {
        outputArr[i] = outputArr[i >> 1]  + (i & 1);
    }
    return outputArr;
};
```


### Submission 2
Runtime 66 ms  | Memory 48.4 MB
This submission was faster than the previous one.

Instead of counting number of 1's in given `i` one by one, 
I could use the number of 1's of the closest pre-calculated number that has the common number of 1's with `i`.
By running `i & (i-1)`, I could "filter" out the 1's of the closest number that has the maximum number of common 1's with `i`. 
`i & (i-1)` could result in `i-1` or some other previous number that doesn't contain 1's of `i-1` not in `i` 
(for example, `1011 & 1100` will result in `1000`. `1000` doesn't contain `0011` that is in `i-1=1011`, but not in `i=1100`).
After that, I just need to add `i`'s additional 1 bit to the number of 1's of the closest number.


```javascript
/**
 * @param {number} n
 * @return {number[]}
 */
var countBits = function(n) {
    const outputArr = [];
    outputArr[0] = 0; 

    for (let i =1; i <= n; i++) {
        outputArr[i] = outputArr[i & (i-1)] + 1;
    }
    return outputArr;
};

```
