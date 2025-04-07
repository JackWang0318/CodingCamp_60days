## 字母异位词分组

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        '''
        对字符串排序后 字母异位词就会归为一类
        '''
        str_dict = {}

        for s in strs:
            # print(sorted(s))  
            # >>> ['a', 'b', 'c' ]
            sorted_s = ''.join(sorted(s))  # 因为字符串是immutable的 不能直接sort(s)
            if sorted_s not in str_dict:
                str_dict[sorted_s] = []
            str_dict[sorted_s].append(s)

        return list(str_dict.values())
```
