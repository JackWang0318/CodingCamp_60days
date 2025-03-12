## 零钱兑换
完全背包 组合数的情况 不强调排列 外遍历物品(确保每个)内遍历背包
- 2025.3.12: 求的是最少的方案方法个数，那么钱币有顺序和没有顺序都可以，都不影响钱币的最小个数，所以内外遍历无所谓
dp数组的含义: dp[j]：凑足总额为j所需钱币的最少个数为dp[j], 最少的话, 递推的时候就会需要用到min
考虑到递推公式的特性，dp[j]必须初始化为一个最大的数，否则就会在min(dp[j - coins[i]] + 1, dp[j])比较的过程中被初始值覆盖。
```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [float('inf')] * (amount + 1)  # 创建动态规划数组，初始值为正无穷大
        dp[0] = 0  # 初始化背包容量为0时的最小硬币数量为0

        for coin in coins:  # 遍历硬币列表，相当于遍历物品
            # print(f'遍历硬币{coin}:')
            # print(f'遍历背包容量前:\n{dp}:')

            for i in range(coin, amount + 1):  # 因为是完全背包, 所以正序遍历背包容量
                if dp[i - coin] != float('inf'):  # 如果dp[i - coin]不是初始值，则进行状态转移
                    dp[i] = min(dp[i - coin] + 1, dp[i])  # 更新最小硬币数量
            # print(f'遍历背包容量后:\n{dp}:')
            
        if dp[amount] == float('inf'):  # 如果最终背包容量的最小硬币数量仍为正无穷大，表示无解
            return -1
        return dp[amount]  # 返回背包容量为amount时的最小硬币数量

```

## 完全平方数
- 2025.3.12：跟零钱兑换一样 求最少方案数
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
- 2025.3.12：TODO 
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
