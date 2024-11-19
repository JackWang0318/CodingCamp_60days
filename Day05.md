## 哈希表
### 《代码随想录》哈希表：有效的字母异位词
#### Easiest Hash table
Directly count  the alphabet counting number, easy.

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        from collections import defaultdict
        
        s_dict = defaultdict(int)
        t_dict = defaultdict(int)
        for x in s:
            s_dict[x] += 1
        
        for x in t:
            t_dict[x] += 1
        return s_dict == t_dict
```


### 《代码随想录》哈希表：两个数组的交集
#### Hash table 
Done on the library's computer, written in English.
```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
    # 使用哈希表存储一个数组中的所有元素
        table = {}
        for num in nums1:
            table[num] = table.get(num, 0) + 1
        #  would result in increasing table[num] by one (and if not exists would by set to one)
        # 使用集合存储结果
        res = set()
        for num in nums2:
            if num in table:
                res.add(num)
                del table[num]
        
        return list(res)
```

### 《代码随想录》哈希表：快乐数
#### Happy Number
当我们遇到了要快速判断一个元素是否出现集合里的时候，就要考虑哈希法了。所以这道题目使用哈希法，来判断这个sum是否重复出现，如果重复了就是return false， 否则一直找到sum为1为止。
```python
class Solution:
   def isHappy(self, n: int) -> bool:
        # sum may be in a cycle
        seen = set()
        while n != 1:
            n = sum(int(i) ** 2 for i in str(n))
            if n in seen:
               return False
            seen.add(n)
        return True
```


### 《代码随想录》哈希表：两数之和
#### Begining of 3 number sum and 4 number sum
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        #创建一个集合来存储我们目前看到的数字
        seen = set()             
        for i, num in enumerate(nums):
            complement = target - num
            if complement in seen:
                return [nums.index(complement), i]
            seen.add(num)
