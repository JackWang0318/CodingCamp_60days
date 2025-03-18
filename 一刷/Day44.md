## 最长递增子序列
- 2025.3.18：注意区分和连续子序列的区别：
- 概括来说：不连续递增子序列的跟前0-i 个状态有关，连续递增的子序列只跟前一个状态有关(所以后者可以用贪心来做,可以直接遍历一遍数组)
![image](https://github.com/user-attachments/assets/031ca579-bd99-4f96-a414-1981718de9ae)

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        '''
            子序列的定义;
            dp[i]表示i之前包括i的以nums[i]结尾的最长递增子序列的长度;
            dp数组初始化
            状态转移方程
        '''
        if len(nums) <= 1:
            return len(nums)
        dp = [1] * len(nums) # 初始化
        result = 1
        for i in range(1, len(nums)):
            for j in range(0, i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j] + 1) 
                    #这里的 dp[j] + 1 表示在以nums[j]结尾的最长递增子序列长度(dp[j])的基础上，
                    #加上 nums[i] 形成的新子序列的长度
            result = max(result, dp[i]) #取长的子序列
        return result
```
## 最长重复子数组
- 注意题目中说的子数组，其实就是连续子序列
- dp数组含义: dp[i][j] ：以下标i - 1为结尾的nums1，和以下标j - 1为结尾的nums2，最长重复子数组长度为dp[i][j]
- 注意初始化的时候最开始的dp[0][0]

![image](https://github.com/user-attachments/assets/a7e91d8f-aea2-4b23-9318-ec7a415bad1f)
```python
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        # 创建一个二维数组 dp，用于存储最长公共子数组的长度
        dp = [[0] * (len(nums2) + 1) for _ in range(len(nums1) + 1)]
        # 记录最长公共子数组的长度
        result = 0
        # 遍历数组 nums1
        for i in range(1, len(nums1) + 1):
            # 遍历数组 nums2
            for j in range(1, len(nums2) + 1):
                # 如果 nums1[i-1] 和 nums2[j-1] 相等
                if nums1[i - 1] == nums2[j - 1]:
                    # 在当前位置上的最长公共子数组长度为前一个位置上的长度加一
                    dp[i][j] = dp[i - 1][j - 1] + 1
                # 更新最长公共子数组的长度
                if dp[i][j] > result:
                    result = dp[i][j]
        # 返回最长公共子数组的长度
        return result

```
## 最长公共子序列 / 变式:不相交的线
- dp含义: dp[i][j]：长度为[0, i - 1]的字符串text1与长度为[0, j - 1]的字符串text2的最长公共子序列为dp[i][j]

![image](https://github.com/user-attachments/assets/d6ec6e47-7bac-40eb-907a-81009f067f4b)

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        # 创建一个二维数组 dp，用于存储最长公共子序列的长度
        dp = [[0] * (len(text2) + 1) for _ in range(len(text1) + 1)]
        
        # 遍历 text1 和 text2，填充 dp 数组
        for i in range(1, len(text1) + 1):
            for j in range(1, len(text2) + 1):
                ###
                if text1[i - 1] == text2[j - 1]:
                    # 如果 text1[i-1] 和 text2[j-1] 相等，则当前位置的最长公共子序列长度为左上角位置的值加一
                    dp[i][j] = dp[i - 1][j - 1] + 1
                else:
                    # 如果 text1[i-1] 和 text2[j-1] 不相等，则当前位置的最长公共子序列长度为上方或左方的较大值
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
                ###
        # 返回最长公共子序列的长度
        return dp[len(text1)][len(text2)]

```

## 最大子序和 (dp方法)
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        dp = [0] * len(nums)
        dp[0] = nums[0]
        result = dp[0]
        for i in range(1, len(nums)):
            dp[i] = max(dp[i-1] + nums[i], nums[i]) #状态转移公式
            result = max(result, dp[i]) #result 保存dp[i]的最大值
        return result
```
## 判断子序列
![image](https://github.com/user-attachments/assets/3bfce085-0fa9-4b2d-a3d6-90d1a9b6e0a3)

![image](https://github.com/user-attachments/assets/e7f19ff4-7c28-4646-a5f3-0ae6cfe9c050)

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        dp = [[0] * (len(t)+1) for _ in range(len(s)+1)]
        for i in range(1, len(s)+1):
            for j in range(1, len(t)+1):
                if s[i-1] == t[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = dp[i][j-1]
        if dp[-1][-1] == len(s):
            return True
        return False
```
