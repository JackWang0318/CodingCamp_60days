## 最小覆盖子串 滑动窗口
- 未优化
```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        def is_window_valid(window_counts, t_counts):
            """检查当前窗口是否包含 t 的所有字符，且频率足够"""
            for char, count in t_counts.items():
                if window_counts.get(char, 0) < count:
                    return False
            return True

        if not s or not t or len(t) > len(s):
            return ""
        # 统计 t 的字符频率
        t_counts = {}
        for char in t:
            t_counts[char] = t_counts.get(char, 0) + 1

        window_counts = {}
        left = 0
        min_len = float('inf')
        result = ""

        for right, char in enumerate(s):
            # 更新当前窗口的字符频率
            window_counts[char] = window_counts.get(char, 0) + 1

            # 如果当前窗口满足条件，尝试收缩左边界
            while is_window_valid(window_counts, t_counts):
                current_len = right - left + 1
                if current_len < min_len:
                    min_len = current_len
                    result = s[left:right + 1]

                # 移动左指针
                left_char = s[left]
                window_counts[left_char] -= 1
                # if window_counts[left_char] == 0:
                #     del window_counts[left_char]
                left += 1
        return result
```
