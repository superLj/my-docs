### 解题思路

`明确 base case -> 明确「状态」-> 明确「选择」 -> 定义 dp 数组/函数的含义`

> [编辑距离](https://leetcode-cn.com/problems/edit-distance/)

```
#伪码框架
 if s1[i] == s2[j]:
     啥都别做（skip）
     i, j 同时向前移动
 else:
     三选一：
         插入（insert）
         删除（delete）
         替换（replace）
```

> [凑零钱问题](https://leetcode-cn.com/problems/coin-change/)

```
# 伪码框架
def coinChange(coins: List[int], amount: int):

    # 定义：要凑出金额 n，至少要 dp(n) 个硬币
    def dp(n):
        # 做选择，选择需要硬币最少的那个结果
        for coin in coins:
            res = min(res, 1 + dp(n - coin))
        return res

    # 题目要求的最终结果是 dp(amount)
    return dp(amount)
```
