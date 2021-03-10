---
title: leetcode-121 Best Time to Buy and Sell Stock
date: 2018-05-03 12:33:54
tags:
        - algorithm
        - array
        - dp
categories: 
	    - leetcode
---

# Best Time to Buy and Sell Stock
## 题目
Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.
**Example 1:**
```
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
```
**Example 2:**
```
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

## 解决方案
```python
class Solution:
    def maxProfit(self, A):
        """
        :type prices: List[int]
        :rtype: int
        """
        if not A: return 0
        mp,lv = 0,A[0]
        for num in A[1:]:
            lv = min(lv,num)
            mp = max(mp,num-lv)
        return mp
```

### 算法思想
只能做一次交易，只需要找到全局的最小和全局的最大，那么就能找到答案

#### 复杂度分析
时间复杂度O(n)  一重循环