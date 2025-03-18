# 买卖股票系列 
- 2025.3.17：买卖股票 对第i天进行操作时 的递推
## I
![image](https://github.com/user-attachments/assets/52b411bc-6029-445d-ad46-0e5dbf7e85f1)

- dp[i][0]第i天持有这支股票的最大收益; dp[i][1]...不持有...
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        length = len(prices)
        if length == 0:
            return 0
        # 初始化
        dp = [[0] * 2 for _ in range(length)]
        dp[0][0] = -prices[0] # 第0天持有
        dp[0][1] = 0 # 第0天不持有
        
        for i in range(1, length):
            dp[i][0] = max(dp[i-1][0], -prices[i])  # 当天 不买入和买入 取max
            dp[i][1] = max(dp[i-1][1], prices[i] + dp[i-1][0]) # 当天 不卖出 和卖出 取max
        # return max(dp[-1][0], dp[-1][1]) 本题中 肯定是后者大
        return dp[-1][1] # 最后一天不持有该股票的最大收益 
```

## II 
- 在I中，因为股票全程只能买卖一次，所以如果买入股票，那么第i天持有股票即dp[i][0]一定就是 -prices[i]；
而本题，因为一只股票可以买卖多次，所以当第i天买入股票的时候，所持有的现金可能有之前买卖过的利润，i.e. dp[i-1][1] - prices[i]
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        length = len(prices)
        dp = [[0] * 2 for _ in range(length)]
        dp[0][0] = -prices[0]
        dp[0][1] = 0
        for i in range(1, length):
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] - prices[i]) #注意这里是和 I 唯一不同的地方
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] + prices[i])
        return dp[-1][1]
```

## III 最多买卖2次
- 这里dp数组的状态需要扩展：
- dp[i]的[0,1,2,3,4]分别表示：
- 不操作，第一次持有，第一次不持有，第二次持有，第二次不持有
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if len(prices) == 0:
            return 0
        dp = [[0] * 5 for _ in range(len(prices))]
        dp[0][1] = -prices[0]
        dp[0][3] = -prices[0]
        for i in range(1, len(prices)):
            dp[i][0] = dp[i-1][0]
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i]) # 第i天保持持有vs第i天买入
            dp[i][2] = max(dp[i-1][2], dp[i-1][1] + prices[i]) # 第i天保持不持有vs第i天卖出
            dp[i][3] = max(dp[i-1][3], dp[i-1][2] - prices[i]) # 第i天保持持有vs第i天(第二次)买入 (dp[i-1][2]由第一次不持有的状态转移)
            dp[i][4] = max(dp[i-1][4], dp[i-1][3] + prices[i]) # 第i天保持持有vs第i天(第二次)卖出
        return dp[-1][4]
```
