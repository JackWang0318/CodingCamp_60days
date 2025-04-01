## 最大正方形 
![image](https://github.com/user-attachments/assets/4f65ac40-6d86-4f80-aaa7-7f4631765d4e)


```python
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        '''
            dp[i][j]: 以matrix[i-1][j-1]为右下角的正方形 的最大边长
        '''
        m,n = len(matrix), len(matrix[0])
        dp = [[0]* (n+1) for _ in range(m+1)]
        max_side = 0
        for i in range(1,m+1):
            for j in range(1, n+1):
                if matrix[i-1][j-1] == '1':
                    dp[i][j] = min(dp[i - 1][j - 1], dp[i - 1][j], dp[i][j - 1]) + 1
                    if max_side < dp[i][j]:
                        max_side = dp[i][j]
        return max_side**2

```
