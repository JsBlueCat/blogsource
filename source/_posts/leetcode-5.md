---
title: leetcode-5 Longest Palindromic Substring
date: 2018-05-01 08:53:48
tags:
        - algorithm
        - string
        - dp  
categories: 
	    - leetcode

---

# 5. Longest Palindromic Substring

## 题目
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

**Example 1:**
```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

**Example 2:**
```
Input: "cbbd"
Output: "bb"
```

## 解决方案

```python
class Solution:
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """
        return self.manacher(s)


    def manacher(self,s):
        ts = '#' + '#'.join(s) + '#'
        print(ts)
        r = [0] * len(ts)
        m ,mr , p ,ind = 0,0,0,0
        for i in range(len(ts)):
            if i<mr:
                r[i] = min(r[2*p-i],mr - i)
            else:
                r[i] = 1

            ## enlarge
            while i - r[i] >=0 and i + r[i]< len(ts) and ts[i-r[i]] == ts[i+r[i]]:
                r[i] +=1

            if i+r[i] - 1 > mr:
                mr = i+r[i] - 1
                p = i

            if m<r[i]:
                m  = r[i]
                ind = i
            else:
                pass
        return ts[ind-m+1:ind+m-1].replace('#','')
```

### 算法思想
本题主要使用的是 马拉车算法 加入#分割字符，然后不断的向外扩展回文串。

```
aba  ———>  #a#b#a#
abba ———>  #a#b#b#a#
```

```
char:    # a # b # a #
 RL :    1 2 1 4 1 2 1
RL-1:    0 1 0 3 0 1 0
  i :    0 1 2 3 4 5 6

char:    # a # b # b # a #
 RL :    1 2 1 2 5 2 1 2 1
RL-1:    0 1 0 1 4 1 0 1 0
  i :    0 1 2 3 4 5 6 7 8
```

![](/images/manacher.png)

如图所示pos是当前回文的中心点，maxright 是回文的扩展位置。

#### 算法分析
主要是扩展字符串的长度 时间复杂度O(n) 空间复杂度O(n)