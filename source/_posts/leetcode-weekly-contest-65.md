---
title: Leetcode Weekly Contest 65 题解及训练
tags: 
    - Leetcode
categories:
    - Algorithm
date: 2018-01-01 00:00:00
mathjax: true
---

## [754. Reach a Number](https://leetcode.com/contest/weekly-contest-65/problems/reach-a-number/)

### 大意

给出一个 `target` 这个数称之为目标数字。我们的其实数字是 `0`，每次可以进行 `+n` 和 `-n` 的操作，这个 `n` 是次数，当我们重复进行这个操作并第一次到达 `target` 的时候，输出 `n`。

### 思路

比赛的时候，首先想到的是 *BFS* 搜，感觉就是一道搜索题。但是 `target` 的数据量是 `[-10^9, 10^9]`，高达 `2e9` 的数量级，搜索肯定是行不通的。再考虑剪枝手段，发现这样的数字正负是对称的，可以减一半的基数，再建立一个 `visited` 的表来标记已经访问的数字，但是复杂度也是相当的大。

第二考虑动态规划，但是在推倒转移方程的时候，发现 `dp[n]` 与 `dp[n - 1]` 没有直接的数量关系，而且整体的关系网图是一个树形结构，所以在 `dp[n]` 的时候，要考虑 `2^n` 种情况，显然*状态压缩行不通*。（比赛GG）

赛后在 *Discuss* 中看到一个数学思路。

* 由于问题具有对称性，所以可以先将 `target` 绝对值，得到 `T`;
* 计算前 n 项和 `sum` 并让其满足下式：

$$sum=1+2+3+...+n\geq T$$

* 如果 $1+2+3+...+n=T$ 直接返回 `n`
* 否则令 $res=sum-T$，如果 `res` 是偶数，那么肯定存在一个整数 `x` 保证 $2x=res$在 $[1,n]$ 中。我们让这个 `x` 取反即可，仍旧返回 `n`

$$res\ is\ odd\Longrightarrow T=1+2+3+...-x+...+n\ \ (x\subseteq [1,n])$$

* 如果 `res` 是奇数且 `n + 1` 是奇数，继续向 `sum` 中添加 `n + 1`。然后得到 `x` 同上一操作进行翻转。如果 `n + 1` 是偶数，那么继续添加 `n + 2` 在做 `x` 翻转操作。由于 $sum\geq T$ 且是 `sum` 的最小值，所以 `n` 能保证最小值。

```python
import math
class Solution:
    def reachNumber(self, target):
        target_abs = abs(target)
        n = math.ceil((-1 + math.sqrt(1 + 8 * target_abs)) / 2)
        sum = n * (n + 1) / 2
        if sum == target:
            return n
        res = int(sum - target)
        if res & 1 == 0:
            return n
        else :
            return n + (1 if n & 1 == 0 else 2)
        
```
