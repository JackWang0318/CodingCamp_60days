## 买卖股票含冷冻期

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        '''
            dp[i][j]表示第i天状态j时的maxProfit
            状态有4种情况:
                0:持有股票
                    当天买入: max(dp[i-1][3] - prices[i], dp[i-1][1] - prices[i])
                    之前买入: dp[i-1][0]
                1:非持有股票 不是当天卖出
                    说明之前就卖出了: max(dp[i-1][1], dp[i-1][3]) 前一天是状态1or3
                2:非持有股票 是当天卖出
                    前一天只可能是状态0 当天卖出: dp[i-1][0] + prices[i]
                3:冷冻期 (前一天的状态只能是状态2)
                    前一天是状态2: dp[i-1][2]
        '''
        n = len(prices)
        dp = [[0] * 4 for _ in range(n)]

        # 初始化
        dp[0][0] = - prices[0] # 第0天持有的情况

        for i in range(1, n):
            dp[i][0] = max(dp[i - 1][0], max(dp[i - 1][3], dp[i - 1][1]) - prices[i])
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][3])
            dp[i][2] = dp[i - 1][0] + prices[i]
            dp[i][3] = dp[i - 1][2]

        maxProfit = max(dp[n-1][1],dp[n-1][2], dp[n-1][3])
        return maxProfit
```
