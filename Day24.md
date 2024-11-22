## 复原IP地址
切割问题 -> 回溯法
注意isValid 判断ip地址是否合法的 功能函数
```python
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        result = []
        self.backtracking(s, 0,0,"",result)
        return result

    def backtracking(self, s, start_index, point_num, current, result):
        if point_num == 3: # .的数量达到三个 即切割次数已到达三次
            if self.isValid(s,start_index,len(s)-1): # 检查剩下的子串是否满足ip地址条件
                current += s[start_index:]
                result.append(current)
            return
        for i in range(start_index, len(s)):
            if self.isValid(s, start_index, i): # 判断子串是否合法
                sub = s[start_index:i+1]
                self.backtracking(s, i+1, point_num+1, current + sub + '.', result) # 自己包含了隐藏回溯
            else:
                break # 剪枝

    def isValid(self, s, start, end):# 判断s[start:end]是否为合法ip地址
        if start > end:
            return False
        if s[start] == '0' and start != end: # 0开头 但不是单独的0 不合法
            return False
        num = 0
        for i in range(start, end + 1):
            if not s[i].isdigit():
                return False
            num =  num * 10 + int(s[i])
            if num > 255:
                return False
        return True

```

## 子集
```python
class Solution:
    def subsets(self, nums):
        result = []
        path = []
        self.backtracking(nums, 0, path, result)
        return result

    def backtracking(self, nums, startIndex, path, result):
        result.append(path[:])  # 收集子集，要放在终止添加的上面，否则会漏掉自己
        if startIndex >= len(nums):  # 终止条件可以不加
            return
        for i in range(startIndex, len(nums)):
            path.append(nums[i])
            self.backtracking(nums, i + 1, path, result)
            path.pop()
```

## 子集II
![image](https://github.com/user-attachments/assets/ff786a89-df49-4971-8c6b-17baec2e4e82)

带有重复元素的情况 要注意树层去重
```python
class Solution:
    def subsetsWithDup(self, nums):
        result = []
        path = []
        used = [False] * len(nums)
        nums.sort()  # 去重需要排序
        self.backtracking(nums, 0, used, path, result)
        return result

    def backtracking(self, nums, startIndex, used, path, result):
        result.append(path[:])  # 收集子集
        for i in range(startIndex, len(nums)):
            # used[i - 1] == True，说明同一树枝 nums[i - 1] 使用过
            # used[i - 1] == False，说明同一树层 nums[i - 1] 使用过
            # 而我们要对同一树层使用过的元素进行跳过
            if i > 0 and nums[i] == nums[i - 1] and not used[i - 1]:
                continue
            path.append(nums[i])
            used[i] = True
            self.backtracking(nums, i + 1, used, path, result)
            used[i] = False
            path.pop()
```
