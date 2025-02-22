## KMP 算法回顾

- 算法本质的目的就是匹配字符串，KMP利用了前缀表的方法 减少了需要匹配的遍历次数
- 主要难点在于: next数组的构造和使用
- Intuition: 当字符串不匹配时，可以利用之前已经匹配过的信息，减少不必要的多余的匹配，提高效率。
```python
class Solution:
    def getNext(self, nxt, s):
        # 本质 利用已经掌握的信息来规避重复的运算 
        # 初始化 前后缀相等 不相等的情况 返回next数组
        nxt[0] = 0
        j = 0 # j标记子串 前缀的末尾位置 同时也是记录了i及i之前子串 最长相等前后缀的长度
        for i in range(1, len(s)): # i标记子串 后缀的末尾位置 从第一个子串开始
            # 处理
            while j > 0 and s[i] != s[j]: # j==0的时候 就不需要回退了
                j = nxt[j - 1] #回退j 至前一位next数组下标对应的位置
            if s[i] == s[j]:
                j += 1 # 前后缀相等 j向后移动
            nxt[i] = j # 记录当前字符对应的最长相等前后缀的长度 j作为 记录了i及i之前子串 最长相等前后缀的长度
        return nxt
    
    def strStr(self, haystack: str, needle: str) -> int:
        if len(needle) == 0:
            return 0
        next = [0] * len(needle)
        self.getNext(next, needle)
        j = 0
        for i in range(len(haystack)):
            while j > 0 and haystack[i] != needle[j]:
                j = next[j - 1]
            if haystack[i] == needle[j]:
                j += 1
            if j == len(needle):
                return i - len(needle) + 1
        return -1
```
    
