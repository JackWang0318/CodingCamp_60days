## 零钱兑换
完全背包 组合数的情况 不强调排列 外遍历物品内遍历背包
```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [float('inf')] * (amount + 1)  # 创建动态规划数组，初始值为正无穷大
        dp[0] = 0  # 初始化背包容量为0时的最小硬币数量为0

        for coin in coins:  # 遍历硬币列表，相当于遍历物品
            # print(f'遍历硬币{coin}:')
            # print(f'遍历背包容量前:\n{dp}:')

            for i in range(coin, amount + 1):  # 遍历背包容量
                if dp[i - coin] != float('inf'):  # 如果dp[i - coin]不是初始值，则进行状态转移
                    dp[i] = min(dp[i - coin] + 1, dp[i])  # 更新最小硬币数量
            # print(f'遍历背包容量后:\n{dp}:')
            
        if dp[amount] == float('inf'):  # 如果最终背包容量的最小硬币数量仍为正无穷大，表示无解
            return -1
        return dp[amount]  # 返回背包容量为amount时的最小硬币数量

```

## 完全平方数
```python
class Solution:
    def numSquares(self, n: int) -> int:
        # 跟零钱兑换 几乎一样 求最少方法数
        # 完全平方数是物品 n为背包容量
        dp = [float('inf')] * (n + 1)
        dp[0] = 0

        for i in range(1, n + 1):  # 遍历背包
            for j in range(1, int(i ** 0.5) + 1):  # 遍历物品
                # 更新凑成数字 i 所需的最少完全平方数数量
                dp[i] = min(dp[i], dp[i - j * j] + 1)

        return dp[n]
```

## 单词拆分
强调排列 所以外遍历背包内遍历物品
```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        wordSet = set(wordDict)
        n = len(s)
        dp = [False] * (n + 1)  # dp[j] 表示字符串的前 j 个字符是否可以被拆分成单词
        dp[0] = True  # 初始状态，空字符串可以被拆分成单词

        for j in range(1, n + 1): # 遍历背包
            for i in range(j): # 遍历单词
                if dp[i] and s[i:j] in wordSet:
                    dp[j] = True  # 如果 s[0:i] 可以被拆分成单词，并且 s[i:j] 在单词集合中存在，则 s[0:j] 可以被拆分成单词
                    break
            # print(dp)
        return dp[n]

```
