---
title: Leetcode Weekly Contest 64 题解及训练
tags: 
    - Leetcode
categories:
    - Algorithm
date: 2017-12-24 00:00:00
---

## [747. Largest Number Greater Than Twice of Others](https://leetcode.com/contest/weekly-contest-64/problems/largest-number-greater-than-twice-of-others/)

### 大意

给出一个数组，问是否其中的最大值是其他值的 2 倍以上，如果是返回最大值的下标，否则返回 `-1`

### 思路

`sorted` 一下，查一值和二值的大小关系即可，切记用乘法，避免 `0` 的问题。

```python
class Solution:
    def dominantIndex(self, nums):
        if len(nums) is 0:
            return -1
        if len(nums) is 1:
            return 0
        _nums = sorted(nums)
        _max = _nums[-1]
        _max2 = _nums[-2]
        if _max2 * 2 <= _max:
            for i in range(0, len(nums)):
                if _max == nums[i]:
                    return i
        else :
            return -1
```

## [752. Open the Lock](https://leetcode.com/contest/weekly-contest-64/problems/open-the-lock/)

### 大意

一个四位的密码锁，已知 `deadends` 为死锁列表，`target` 为秘钥。一次只能对一位旋转一格。在不途径 `deadends` 列表中的值，求从 `0000` 转到 `target` 需要的步数。如果无法转到，返回 `-1`。

### 思路

*BFS* 搜就好了，**注意剪枝**！！！

剪枝点如下：

* 1. `deadents` 用 `set` 封装一下，让搜索速度变成 `O(logn)`；
* 2. 记录标记表 `visited` 去重；

```python
from collections import deque
class Solution:
    def openLock(self, deadends, target):
        deadents_filter = set(deadends)
        step, end_flag = 0, 'e'
        visited, queue = set(), deque(['0000', end_flag])

        while queue:
            code = queue.popleft()
            # print(code)
            if code == target:
                return step
            if code in visited or code in deadents_filter:
                continue
            if code == end_flag and not queue:
                return -1
            if code == end_flag:
                queue.append(end_flag)
                step += 1
            else:
                visited.add(code)
                queue.extend(self.nextList(code))
        return -1

    def nextList(self, code):
        res = []
        for ind, let in enumerate(code):
            num = int(let)
            res.append(code[:ind] + str(9 if num == 0 else num - 1) + code[ind + 1:])
            res.append(code[:ind] + str(0 if num == 9 else num + 1) + code[ind + 1:])
        return res
```

# Weekly Practice


## [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/description/)

### 大意

题目的意思是，数组中的每个数对应一条线段的长度，索引对应 x 坐标，两个索引可以组成一个底部的宽，高度就是前面所说的线段的长度，而既然是要盛水，高度就是两个线段中较短的一个。

### 思路

*Two Pointer* 向中间扫一遍贪心即可，这题应该是 *Two Pointer* 典型题。

```python
class Solution:
    def maxArea(self, height):
        l, r = 0, len(height) - 1
        res = 0
        while l < r:
            res = max(res, (r - l) * min(height[l], height[r]))
            if height[l] < height[r]:
                l += 1
            else :
                r -= 1
        return res
```
