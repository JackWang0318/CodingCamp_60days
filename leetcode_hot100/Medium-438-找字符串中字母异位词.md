## 438 滑动窗口
- 滑动窗口的核心要点：
1.维护一个有条件的滑动窗口；
2.右端点右移，导致窗口扩大，是不满足条件的罪魁祸首；
3.左端点右移目的是为了缩小窗口，重新满足条件

- 基本所有不定长滑动都可以套这个模板：
本题中，
维护一个输入字符串中字符出现次数-待匹配字符串中字符出现次数的窗口，这个窗口用哈希表来维护。
因为窗口是在输入字符串上滑动的；
在窗口每次扩大，新增一个输入字符串中字符出现次数，哈希表对应字符次数-1；
窗口每次缩小，减少一个输入字符串中字符出现次数，哈希表对应字符次数+1；
```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        ans = []
        cnt_p = Counter(p)  # 统计 p 的每种字母的出现次数
        cnt_s = Counter()  # 统计 s 的长为 len(p) 的子串 s' 的每种字母的出现次数
        for right, c in enumerate(s):
            # 定长 window_len = len(p)
            cnt_s[c] += 1  # 右端点字母进入窗口
            left = right - len(p) + 1
            if left < 0:  # 窗口长度不足 len(p)
                continue
            if cnt_s == cnt_p:  # s' 和 p 的每种字母的出现次数都相同
                ans.append(left)  # s' 左端点下标加入答案
            cnt_s[s[left]] -= 1  # 左端点字母离开窗口
        return ans

```
