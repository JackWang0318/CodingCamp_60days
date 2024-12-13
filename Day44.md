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

## 不相交的线

## 最大子序和

## 判断子序列
