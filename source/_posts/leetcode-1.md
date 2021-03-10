---
title: leet-code-1 Two Sum
date: 2018-04-29 08:55:09
tags:
        - algorithm
        - hash_table
categories: 
	    - leetcode        
---

# Two Sum
## 题目
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

## 解决方案
```python
class Solution(object):
    def twoSum(self, nums, target):
        if len(nums) <= 1:
            return False
        buff_dict = {}
        for i in range(len(nums)):
            if nums[i] in buff_dict:
                return [buff_dict[nums[i]], i]
            else:
                buff_dict[target - nums[i]] = i
```

### 算法思想
使用hash_table来记录。a+b=c 我们记录 c-a 然后每次的b和table中的比较。这样就会发现了。
#### 算法分析
时间复杂度O(n),hash_table底层是红叉树，算法的复杂度是 O(log\\(\_m\\)p) 其中m,p均为常数