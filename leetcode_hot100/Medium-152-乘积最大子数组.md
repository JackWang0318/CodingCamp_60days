## 乘积最大子数组
![image](https://github.com/user-attachments/assets/e7f09afc-ca95-47c7-beb1-ec7050e2ea66)
```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:

         # 注意答案可能是负数
        ans = -inf  # 注意答案可能是负数
        f_max = f_min = 1
        for x in nums:
            f_max, f_min = max(f_max * x, f_min * x, x), \
                           min(f_max * x, f_min * x, x)
            ans = max(ans, f_max)
        return ans


        # n = len(nums)
        # f_max = [0] * n #
        # f_min = [0] * n # 要存储最小的负数 可能负负得正
        # max_res = f_max[0] = f_min[0] = nums[0]
        # for i in range(1, n):
        #     x = nums[i]
        #     # 把 x 加到右端点为 i-1 的（乘积最大/最小）子数组后面，
        #     # 或者单独组成一个子数组，只有 x 一个元素
        #     f_max[i] = max(f_max[i - 1] * x, f_min[i - 1] * x, x)
        #     f_min[i] = min(f_max[i - 1] * x, f_min[i - 1] * x, x) #
        #     max_res = max(max_res, f_max[i])
        # return max_res
```
