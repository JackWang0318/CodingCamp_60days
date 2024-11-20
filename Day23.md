## 组合总和
注意剪枝操作之前，不能先更新total 因为如果先更新total然后发现要break了 total会被误更新。
然后剪枝版本 传入的candidates是sorted过的 
```python
class Solution:
    def __init__(self):
        self.result = []

    def backtracking(self, candidates, target, total, startindex, path, result):
        if total == target:
            result.append(path[:])
            return

        for i in range(startindex, len(candidates)):
            # 注意此处 不能先给total加上candidates[i] 因为你先加上 然后发现要剪枝 就会误更新total了
            if total + candidates[i] > target: # 横向剪枝
                break
            total += candidates[i]
            path.append(candidates[i])
            self.backtracking(candidates, target, total, i,path,result)
            total -= candidates[i]
            path.pop()


    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        candidates.sort()  # 需要排序 才能配合剪枝的逻辑
        self.backtracking(candidates,target, 0,0, [],self.result)
        return self.result

```

## 组合总和2
![image](https://github.com/user-attachments/assets/262e6928-03f9-49e2-8c2b-1dfbd31c5ab5)

```python
class Solution:
    def __init__(self):
        self.result = []

    def backtracking(self, candidates, target, used, total, startindex, path, result):
        if total == target:
            result.append(path[:])
            return

        for i in range(startindex, len(candidates)):
            # 去重的是 “同一树层上的使用过”，如何判断同一树层上元素（相同的数值）是否使用过了呢。如果candidates[i] == candidates[i - 1] 并且 used[i - 1] == false，就说明：前一个树枝，使用了candidates[i - 1]，也就是说同一树层使用过candidates[i - 1]。
            if i > startindex and candidates[i] == candidates[i - 1] and not used[i - 1]:
                continue
            if total + candidates[i] > target: # 横向剪枝
                break

            used[i] = True
            total += candidates[i]
            path.append(candidates[i])
            self.backtracking(candidates, target, used, total, i + 1, path, result)
            used[i] = False
            total -= candidates[i]
            path.pop()


    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:

        used = [False] * len(candidates) # 记录每个元素是否used
        candidates.sort()  # 需要排序 才能配合剪枝的逻辑
        self.backtracking(candidates,target,used, 0,0, [],self.result)
        return self.result
```
## 分割回文字符串
- 切割问题也可以等效为一种组合
- 卡哥列出如下几个难点：
切割问题可以抽象为组合问题
;如何模拟那些切割线
;切割问题中递归如何终止
;在递归循环中如何截取子串
;如何判断回文
![image](https://github.com/user-attachments/assets/e48283a4-bd47-451e-bb85-2c95d0adca2b)
```python
class Solution:

    def partition(self, s: str) -> List[List[str]]:
        '''
        递归用于纵向遍历
        for循环用于横向遍历
        当切割线迭代至字符串末尾，说明找到一种方法
        类似组合问题，为了不重复切割同一位置，需要start_index来做标记下一轮递归的起始位置(切割线)
        '''
        result = []
        self.backtracking(s, 0, [], result)
        return result

    def backtracking(self, s, start_index, path, result ):
        # Base Case
        if start_index == len(s):
            result.append(path[:])
            return
        
        # 单层递归逻辑
        for i in range(start_index, len(s)):
            # 此次比其他组合题目多了一步判断：
            # 判断被截取的这一段子串([start_index, i])是否为回文串
            if self.is_palindrome(s, start_index, i):
                path.append(s[start_index:i+1])
                self.backtracking(s, i+1, path, result)   # 递归纵向遍历：从下一处进行切割，判断其余是否仍为回文串
                path.pop()             # 回溯


    def is_palindrome(self, s: str, start: int, end: int) -> bool: # 双指针判断法 待优化 可以用dp
        i: int = start        
        j: int = end
        while i < j:
            if s[i] != s[j]:
                return False
            i += 1
            j -= 1
        return True 

```
