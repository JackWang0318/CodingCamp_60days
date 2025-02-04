## 最长递增子序列
![image](https://github.com/user-attachments/assets/031ca579-bd99-4f96-a414-1981718de9ae)

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        if len(nums) <= 1:
            return len(nums)
        dp = [1] * len(nums)
        result = 1
        for i in range(1, len(nums)):
            for j in range(0, i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j] + 1)
            result = max(result, dp[i]) #取长的子序列
        return result
```
## 最长重复子数组
![image](https://github.com/user-attachments/assets/73f138fe-3295-449e-a44a-c438423f6e11)

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
