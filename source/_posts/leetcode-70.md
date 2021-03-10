---
title: leetcode-70. Climbing Stairs
date: 2018-05-03 12:28:44
tags:
        - algorithm
        - array
        - dp 
categories: 
	    - leetcode
	     
---


# Climbing Stairs
## 题目
You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?
**Example 1:**
```
Input: 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```
**Example 2:**
```
Input: 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

## 解决方案
```python
class Solution:
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """
        a = [1,2]
        for i in range(2,n):
            a.append(a[i-1]+a[i-2])
        return a[n-1]
```

### 算法思想
an = an-1 + an-2

#### 复杂度分析
时间复杂度O(n)  是一个斐波那契数列，如果 想要压复杂度可以见我的快速矩阵幂