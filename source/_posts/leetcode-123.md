---
title: leetcode-123 Best Time to Buy and Sell Stock
date: 2018-05-05 07:36:03
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

Design an algorithm to find the maximum profit. You may complete at most two transactions.
**Example 1:**
```
Input: [3,3,5,0,0,3,1,4]
Output: 6
Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
             Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.
```
**Example 2:**
```
Input: [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
             Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are
             engaging multiple transactions at the same time. You must sell before buying again.
```
**Example 3:**
```
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```
## 解决方案
```python
class Solution:
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        if len(prices) <  2 : return 0
        p = []
        ci , m  = prices[0],0
        for i in prices:
            ci = min(ci,i)
            m = max(m,i-ci)
            p.append(m)

        t,cm ,m= 0,prices[-1],0
        for i in range(len(prices)-1, -1, -1):
            cm = max(cm,prices[i])
            m = max(m,cm - prices[i])
            t = max(t,m + p[i])
        return t
```

### 算法思想
只做2次交易，第一次dp把之前的每天交易一次的最大值记录下来。之后自底向上去计算一次当前+之后的收益最大

#### 复杂度分析
时间复杂度O(n)  一重循环