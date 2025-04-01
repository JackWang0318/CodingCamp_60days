## product Except self
- 利用O(n)的时间先把前后缀乘积信息存好
- 之后直接输出答案
```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        '''
            利用前缀和后缀
            pre[i] 表示i左边元素的乘积
            suf[i] 表示i右边元素的乘积
            那么result[i] = pre[i] * suf[i]

            eg:
            nums = [...] 长度为n
            -------------------------------------------------------------------------
            index   0   1  ...  k-1         k                           k+1  ... n-1
            pre     1                      pre[k] = pre[k-1] * nums[k]
            suf                            suf[k] = suf[k+1] * nums[k]              1
        '''
        n = len(nums)
        # 主要是要初始化pre[0]=suf[n-1]=1
        pre = [1] * n
        suf = [1] * n

        for i in range(1, n):
            pre[i] = pre[i - 1] * nums[i - 1]

        for i in range(n - 2, -1, -1):
            suf[i] = suf[i + 1] * nums[i + 1]

        result = [p * s for p, s in zip(pre, suf)]
        return result 
```
