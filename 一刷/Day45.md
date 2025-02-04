## 不同的子序列
dp[i][j]: 表示s[0:i-1]中有多少个t[0:j-1]
![image](https://github.com/user-attachments/assets/b7a057b1-5bac-4034-bb57-7e53eb0df057)
![image](https://github.com/user-attachments/assets/f0da66fe-4629-40c3-8ea0-7f1394c027cb)

```python
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        dp = [[0] * (len(t)+1) for _ in range(len(s)+1)]
        for i in range(len(s)+1):
            dp[i][0] = 1
        for i in range(1, len(s)+1):
            for j in range(1,len(t)+1):
                if s[i-1] == t[j-1]:
                    dp[i][j] = dp[i-1][j-1] + dp[i-1][j]
                else:
                    dp[i][j] = dp[i-1][j]
        return dp[-1][-1]
```

## 两个字符串的删除操作
最开始我想到的也是这个：只要求出两个字符串的最长公共子序列长度即可，那么除了最长公共子序列之外的字符都是必须删除的，最后用两个字符串的总长度减去两个最长公共子序列的长度就是删除的最少步数。
另一种思路是直接就来
dp table 表示的是word1[0:i-1]和word2[0:j-1]之间最少需要多少次删除操作才能变相同
![image](https://github.com/user-attachments/assets/a985ed03-064a-4d05-b579-20b8ba044996)

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp = [[0] * (len(word2)+1) for _ in range(len(word1)+1)]
        # 初始化
        for i in range(len(word1)+1):
            dp[i][0] = i
        for j in range(len(word2)+1):
            dp[0][j] = j
        # 开始遍历
        for i in range(1, len(word1)+1):
            for j in range(1, len(word2)+1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i-1][j-1] + 2, dp[i-1][j] + 1, dp[i][j-1] + 1)
        return dp[-1][-1]
```

## 编辑距离
![image](https://github.com/user-attachments/assets/1b1ba910-bbf4-4bd7-b255-a47ffac3120a)

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        # 创建一个二维dp数组 
        # dp[i][j]: 表示以下标i-1为结尾的字符串word1，和以下标j-1为结尾的字符串word2，最近编辑距离为dp[i][j]
        # 为什么是i-1和j-1? ---方便初始化 
        dp = [[0] * (len(word2)+1) for _ in range(len(word1)+1)]
        for i in range(len(word1)+1):
            dp[i][0] = i
        for j in range(len(word2)+1):
            dp[0][j] = j
        
        # 那dp[0][0]是什么含义呢？总不能是以下标-1为结尾的A数组吧;
        # 其实dp[i][j]的定义也就决定着，我们在遍历dp[i][j]的时候i和j都要从1开始。
        for i in range(1, len(word1)+1):
            for j in range(1, len(word2)+1):
                if word1[i-1] == word2[j-1]: # 不操作
                    dp[i][j] = dp[i-1][j-1]
                else: # 增 删 换
                    dp[i][j] = min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1]) + 1
        return dp[-1][-1]
```
