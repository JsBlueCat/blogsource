---
title: leet-code-4 Median of Two Sorted Arrays
date: 2018-04-29 08:55:09
tags:
        - algorithm
        - array
        - binary_search
categories: 
	    - leetcode        
---

# Median of Two Sorted Arrays
## 题目
There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

**Example 1:**
```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```
**Example 2:**
```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```
## 解决方案
```python
class Solution:
    def findMedianSortedArrays(self, A, B):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        l = len(A) + len(B)
        if l%2 !=0:
            return self.kth(A,B,l//2)
        else:
            return (self.kth(A,B,l//2) + self.kth(A,B,l//2 -1))/2
    def kth(self,a,b,k):
        if not a: return b[k]
        if not b: return a[k]
        ia,ib = len(a)//2, len(b)//2
        ma,mb = a[ia],b[ib]
        if ia+ib < k:
            if ma > mb:
                return self.kth(a,b[ib+1:],k-ib-1)
            else:
                return self.kth(a[ia+1:],b,k-ia-1)
        else:
            if ma>mb:
                return self.kth(a[:ia],b,k)
            else:
                return self.kth(a,b[:ib],k)
            
```

### 算法思想
主要是递归的思想，找到第k的数，

          left_part          |        right_part
    A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]
    B[0], B[1], ..., B[j-1]  |  B[j], B[j+1],
如图所示 我们找到两边的中值，去比较，然后扔掉小的左半部分，扔掉大的右半部分。维护好k所在的位置。
#### 算法分析
时间复杂O(n+m) 空间复杂O(n+m)
