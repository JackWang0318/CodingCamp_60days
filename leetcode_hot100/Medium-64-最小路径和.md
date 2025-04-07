## dp基础题
- 找递推公式
- 初始化
- 遍历顺序

```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        # @cache  # 缓存装饰器，避免重复计算 dfs 的结果（记忆化）
        # def dfs(i: int, j: int) -> int:
        #     if i < 0 or j < 0:
        #         return inf
        #     if i == 0 and j == 0:
        #         return grid[i][j]
        #     return min(dfs(i, j - 1), dfs(i - 1, j)) + grid[i][j]
        # return dfs(len(grid) - 1, len(grid[0]) - 1)
        m, n = len(grid), len(grid[0])
        dp = [[inf]*(n+1) for _ in range(m+1)]
        # dp的第0行和第0列设为无穷大 作为初始化 
        for i in range(m):
            for j in range(n):
                if i == 0 and j == 0:
                    dp[1][1] = grid[0][0]
                else:
                    dp[i+1][j+1] = min(dp[i][j+1], dp[i+1][j]) + grid[i][j]    
        return dp[m][n]
```
