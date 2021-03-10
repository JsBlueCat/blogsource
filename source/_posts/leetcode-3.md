---
title: leet-code-3 Longest Substring Without Repeating Characters
date: 2018-04-29 08:55:09
tags:
        - algorithm
        - hash_table
        - string
categories: 
	    - leetcode        
---

# Longest Substring Without Repeating Characters
## 题目
Given a string, find the length of the longest substring without repeating characters.

Examples:

Given **"abcabcbb"**, the answer is **"abc"**, which the length is 3.

Given **"bbbbb"**, the answer is **"b"**, with the length of 1.

Given **"pwwkew"**, the answer is **"wke"**, with the length of 3. Note that the answer must be a substring, **"pwke"** is a subsequence and not a substring.


## 解决方案
```python
class Solution:
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        sr = m = 0
        u = {}
        for i in range(len(s)):
            if s[i] in u and sr<= u[s[i]]:
                sr = u[s[i]] + 1
            else:
                m  = max (m,i-sr+1)
            u[s[i]] = i
        return m
```

### 算法思想
使用hash_table来记录。abc 我们记录下标 然后每次的数我们看它是否在table里面，如果遇见重复的就维护好起始串的位置，求出最大的长度
#### 算法分析
时间复杂度O(n),hash_table底层是红叉树，算法的复杂度是 O(log\\(\_m\\)p) 其中m,p均为常数