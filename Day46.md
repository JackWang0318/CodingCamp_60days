### 回文 子串 
- 因为是判断回文，dp[i][j] 记录字符串范围[i,j]（注意是左闭右闭）是否为回文串的布尔值；其依赖于dp[i+1][j-1]是true且s[i]==s[j]， dp[i][j]才为true。
- 初始化要根据遍历顺序进行判断，根据dp递推公式，应该是从左下到右上的顺序进行遍历：
![image](https://github.com/user-attachments/assets/9d1afa16-1820-4524-8de2-9daf72ce5997)

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        dp = [[False] * (len(s)) for _ in range(len(s))]
        result = 0
        for i in range(len(s)-1, -1, -1):
            for j in range(i, len(s)):  # 此处j从i开始遍历
                if s[i]==s[j]:
                    if (j - i) <= 1:
                        result += 1
                        dp[i][j] = True 
                    elif dp[i+1][j-1]:
                        result += 1
                        dp[i][j] = True 
        return result
```

### 最长回文 子序列
- 回文子串是要连续的，回文子序列可不是连续的！
- dp[i][j]：字符串s在[i, j]范围内最长的回文子序列的长度为dp[i][j]。
- 
![image](https://github.com/user-attachments/assets/a509e71f-f8db-4b5d-80d4-c55039fc44ee)

```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        dp = [[0] * len(s) for _ in range(len(s))]
        for i in range(len(s)):
            dp[i][i] = 1
            
        for i in range(len(s)-1, -1 ,-1):
            for j in range(i+1, len(s)):
                if s[i] == s[j]:
                    dp[i][j] = dp[i+1][j-1] + 2
                else:
                    dp[i][j] = max(dp[i+1][j], dp[i][j-1])
        return dp[0][-1]
```
