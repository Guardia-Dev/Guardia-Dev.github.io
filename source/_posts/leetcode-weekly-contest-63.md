---
title: Leetcode Weekly Contest 63 题解及训练
tags: 
    - Leetcode
categories:
    - Algorithm
date: 2017-12-19 00:00:00
---

## [746. Min Cost Climbing Stairs My SubmissionsBack to Contest](https://leetcode.com/contest/weekly-contest-63/problems/min-cost-climbing-stairs/)

### 大意

给出一个序列，代表每一阶到楼梯需要消耗的体力值。但是在爬楼梯的时候，我们可以一次爬一阶，也可以一次爬两阶，求爬到顶楼时消耗的最小体力

### 思路

很明显的一维 DP，状态转移就是下一次所需要消耗的最小体力总和。转移到 `[index - 1]` 和 `[index-2]` 再取一下 `min` 则可以解得。另外状态转移如下：


```
dp[i] = min(dp[i - 1], dp[i - 2] + cost[i])
```


```python
class Solution:
    def minCostClimbingStairs(self, cost):
        dp = [cost[0], cost[1]]
        for i in range(2, len(cost)):
            dp.append(min(dp[i - 1], dp[i - 2]) + cost[i])
        return min(dp[-1], dp[-2])
```

## [748. Shortest Completing Word](https://leetcode.com/contest/weekly-contest-63/problems/shortest-completing-word/)

### 大意

给一个字符串 `licensePlate`，后面给出多个单词，找出第一个单词其中的字母可以拼成 `licensePlate` 这样的串，且格式和字母种类都足够。

### 思路

暴力枚举即可。但是 `licensePlate` 没有规定字符类型，先进行一次剪枝，这里我用的正则来进行剪枝(感觉有点黑科技)。

```python
class Solution:
    def shortestCompletingWord(self, licensePlate, words):
        lower_license = licensePlate.lower()
        filter_license = collections.Counter(re.sub('[^a-zA-Z]', '', lower_license))
        ans = '$' * 1001
        for word in words:
            counter_word = collections.Counter(word)
            len_max_case = (len(word) < len(ans))
            counter_case = all(filter_license[key] <= counter_word[key] for key in filter_license)
            if len_max_case and counter_case:
                ans = word
        return ans
```


# Weekly Practice

## [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)

### 大意

给出一个数字串，将其映射到电话键盘上，返回所有可能打出的字母组合。

### 思路

典型的搜索题，第一反应想多了 DFS，但是考虑到递归的 *StackOverFlow* 问题用 BFS 维护一个队列较为合理。做完之后看题解发现 DFS 也能通过，说明数据量不是很大。

```python
class Solution:
    def letterCombinations(self, digits):
        """
        :type digits: str
        :rtype: List[str]
        """
        if not digits: 
            return []
        
        dict = {}
        dict[2] = ['a', 'b', 'c']
        dict[3] = ['d', 'e', 'f']
        dict[4] = ['g', 'h', 'i']
        dict[5] = ['j', 'k', 'l']
        dict[6] = ['m', 'n', 'o']
        dict[7] = ['p', 'q', 'r', 's']
        dict[8] = ['t', 'u', 'v']
        dict[9] = ['w', 'x', 'y', 'z']
        
        ret = []

        for i in range(0, len(digits)):
            index = int(digits[i])
            if i is 0:
                for let in dict[index]:
                    ret.append(str(let))
            else:
                while len(ret[-1]) is i:
                    item = ret.pop()
                    for let in dict[index]:
                        ret.insert(0, item + let)
        return ret
```


