## 最长连续序列
- 用时间复杂度O(n)来判断数组中最长连续序列的个数
- 哈希表
```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        '''
            时间复杂度为O(n) 想到哈希
        '''
        ans = 0
        st = set(nums)
        for start in st:
            if start - 1 in st: # 之前遍历过 直接跳过
                continue
            end = start + 1
            while end in st: 
                end += 1 # 看end+1是否也在st中
            ans = max(ans, end - 1 - start + 1)
        return ans
```
