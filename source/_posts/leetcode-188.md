---
title: leetcode-188 Best Time to Buy and Sell Stock IV
date: 2018-05-15 08:24:25
tags:
        - algorithm
        - array
        - dp
categories: 
	    - leetcode
---


# Best Time to Buy and Sell Stock IV
## 题目
Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete at most k transactions.
**Example 1:**
```
Input: [2,4,1], k = 2
Output: 2
Explanation: Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.
```
**Example 2:**
```
Input: [3,2,6,5,0,3], k = 2
Output: 7
Explanation: Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4.
             Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
```

## 解决方案
```python
class Solution:
    def maxProfit(self, k, prices):
        """
        :type k: int
        :type prices: List[int]
        :rtype: int
        """
        n = len(prices)
        if k>=n//2:return sum(i-j for i,j in zip(prices[1:],prices[0:-1]) if i>j)
        dp = [[0] * n for _ in range(k+1)]
        for i in range(1,k+1):
            l = [0] * n
            for j in range(1,n):
                p = prices[j] -prices[j-1]
                l[j] = max(dp[i-1][j-1] + max(0,p),l[j-1]+p)
                dp[i][j] = max(dp[i][j-1],l[j])
        return dp[-1][-1]
```

### 算法思想
因为有k段 所以 开一个dp 二维  分别记录 天数和次数的影响，但是这还不够，因为当第k次的时候 超过就不能交易了，所以还要加一个l矩阵
来维护好不能超过第k次

#### 复杂度分析
O(n^2)