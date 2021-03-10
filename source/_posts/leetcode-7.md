---
title: leetcode-7 Reverse Integer
date: 2018-05-01 09:05:52
tags:
        - algorithm
categories:
        - leetcode
---

# Reverse Integer
## 题目
Given a 32-bit signed integer, reverse digits of an integer.
**Example 1:**
```
Input: 123
Output: 321
```
**Example 2:**
```
Input: -123
Output: -321
```
**Example 3:**
```
Input: 120
Output: 21
```

## 解决方案
```javascript
/**
 * @param {number} x
 * @return {number}
 */
var reverse = function(x)
 {
    var sign= (x>0)?1: -1;
    x=Math.abs(x);
    var str=x.toString().split("").reverse().join("");
    var result=sign * Number(str);
    if(result>2147483647 || result < -2147483648)return 0;
    else return result;
};
```

### 算法思想
将数字转化成字符串，再逆置输出，主要考虑爆int

#### 复杂度分析
时间复杂度O(n) 库函数的reverse 是 一重遍历