## 戳气球 区间DP
- dp[i][j] = x 表示，戳破气球 i 和气球 j 之间（开区间，不包括 i 和 j）的所有气球，可以获得的最高分数为 x。
- 如果在(i,k)开区间内 最后一个戳破气球 k，dp[i][j] 的值应该为：dp[i][j] = dp[i][k] + dp[k][j] + points[i]*points[k]*points[j]

![image](https://github.com/user-attachments/assets/bed6016d-983d-4916-a513-49a2b42c2daf)

![image](https://github.com/user-attachments/assets/986cabd1-37c2-4904-80a4-d2c3469600e5)

```python
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        '''
            dp[i][j] = x 表示，戳破气球 i 和气球 j 之间（开区间，不包括 i 和 j）的所有气球，可以获得的最高分数为 x
            如果在(i,k)开区间内 最后一个戳破气球 k，dp[i][j] 的值应该为：
            dp[i][j] = dp[i][k] + dp[k][j] + points[i]*points[k]*points[j]

        '''
        n = len(nums)
        points = [1,*nums,1]
        # # Base Case: dp[i][j] = 0 when j == i+1
        dp = [[0]* (n+2) for _ in range(n+2) ]

        for i in range(n+1,-1,-1):
            for j in range(i+1, n+2):
                for k in range(i+1, j):
                    dp[i][j] = max(dp[i][j], dp[i][k] + dp[k][j] + points[i]*points[k]*points[j])
        return dp[0][-1]
```
