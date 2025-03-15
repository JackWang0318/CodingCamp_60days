## 非递减子序列 
- 2025.3.14：
对树层去重和树枝去重 进一步理解了
![image](https://github.com/user-attachments/assets/1f3e4909-fc88-4826-8851-0e7acb8aa9c0)

```python
class Solution:
    def findSubsequences(self, nums):
        result = []
        path = []
        self.backtracking(nums, 0, path, result)
        return result
    
    def backtracking(self, nums, startIndex, path, result):
        if len(path) > 1:
            result.append(path[:])  # 注意要使用切片将当前路径的副本加入结果集
            # 注意!!! 这里不要加return，因为要取树上的节点 而不是到了叶子结点之类的
        
        uset = set()  # 使用集合对本层元素进行去重
        for i in range(startIndex, len(nums)):
            if (path and nums[i] < path[-1]) or nums[i] in uset:  # 跳过 递减 或者 树层去重的情况
                continue
            
            uset.add(nums[i])  # 记录这个元素在本层用过了，本层后面不能再用了
            path.append(nums[i])
            self.backtracking(nums, i + 1, path, result)
            path.pop()

```


## 全排列
![image](https://github.com/user-attachments/assets/0525b360-84f0-406a-8957-a624f2296836)

和之前一些题目最大的差别在于 不能用start_index来控制递归位置了，而是通过判断used数组是否使用过来 遍历元素
```python
class Solution:
    def backtracking(self, nums, used, path, result):
        if len(path) == len(nums):
            result.append(path[:])  # 注意加[:]
            return # 注意return
        for i in range(len(nums)):
            if used[i]: #使用过 跳过
                continue
            used[i] = True
            path.append(nums[i])
            self.backtracking(nums, used, path, result)
            used[i] = False
            path.pop()

    def permute(self, nums: List[int]) -> List[List[int]]:
        path = []
        result = []
        used = [False] * len(nums)
        self.backtracking(nums, used, path, result)
        return result
```

## 全排列2
![image](https://github.com/user-attachments/assets/96f9876e-6c96-4555-8528-54f7dd656127)
```python
class Solution:
    def permuteUnique(self, nums):
        nums.sort()  # 排序
        result = []
        self.backtracking(nums, [], [False] * len(nums), result)
        return result

    def backtracking(self, nums, path, used, result):
        if len(path) == len(nums):
            result.append(path[:])
            return
        for i in range(len(nums)):
            if (i > 0 and nums[i] == nums[i - 1] and not used[i - 1]) or used[i]: # 作为是否需要筛选的判断
                continue
            used[i] = True
            path.append(nums[i])
            self.backtracking(nums, path, used, result)
            path.pop()
            used[i] = False

```
