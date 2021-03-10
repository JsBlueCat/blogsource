---
title: leetcode-53 Maximum Subarray
date: 2018-05-01 09:13:21
tags:
        - algorithm
        - array
        - dp
categories: 
	    - leetcode
	      
---

# Maximum Subarray
## 题目
Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

**Example:**
```
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

### 解决方案
```python
class Solution:
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        cs,s,e,cn,ma = 0,0,1,nums[0],nums[0]
        for i in range(len(nums)):
            if i == 0:continue
            if nums[i] + cn >= nums[i]:
                cn = cn + nums[i]
            else:
                cn = nums[i]
                cs = i
            if ma > cn:
                pass
            else:
                ma  = cn
                s = cs
                e  = i + 1
        # print(s,e,ma,nums[s:e])
        return ma
        
```

### 算法思想
主要是考虑贪心算法，本题是求解最长连续子段和，使用的就是贪心，如果前面的加当前的是正的我们就认为他是有益的,否则就是有害的,从当前开始算

### 复杂度分析
时间复杂度  主要是 用了一个一维的循环  时间复杂度在 O(n)