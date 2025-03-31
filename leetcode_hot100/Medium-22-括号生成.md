## 括号生成

```python
import functools

# def log_function_calls(func):
#     @functools.wraps(func)
#     def wrapper(*args, **kwargs):
#         print(f"开启回溯 {func.__name__} with args: {args}")
#         func(*args, **kwargs)
#         print(f"结束回溯 {func.__name__} with args: {args}")
#     return wrapper
    

class Solution:
    def generateParenthesis(self, n: int):
        '''
            -右括号的数量不能超过左括号的数量
            -左括号的数量不能超过 n
        '''
        res = []
        
        # @log_function_calls
        def backtrack(path, left, right):
            '''
                path: 当前节点
                left: 左括号数量
                right 右括号数量
            '''
            if len(path) == 2 * n:
                res.append(''.join(path))
                return
            if left < n:
                path.append('(')
                backtrack(path, left+1, right)
                path.pop()
            if right < left:
                path.append(')')
                backtrack(path, left, right+1)
                path.pop()
        backtrack([], 0, 0)
        return res

```
