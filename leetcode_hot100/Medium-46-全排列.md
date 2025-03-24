## 全排列
- 回溯 加used数组标记元素是否被使用

```python
class Solution:
    def backtracking(self, nums, used, path, result): 
        '''
            1. 确定function signiture 递归函数的参数和返回类型
        '''
        # 2. 递归函数的return条件判断
        if len(path) == len(nums):
            result.append(path[:])
            return 
        # 3. 本树层横向遍历
        for i in range(len(nums)):
            if used[i]: #使用过 跳过
                continue
            used[i] = True
            path.append(nums[i])
            # 递归 纵向遍历 
            self.backtracking(nums, used, path, result)
            # 回溯
            used[i] = False
            path.pop()

    def permute(self, nums: List[int]) -> List[List[int]]:
        path = []
        result = []
        used = [False] * len(nums)
        self.backtracking(nums, used, path, result)
        return result
```
