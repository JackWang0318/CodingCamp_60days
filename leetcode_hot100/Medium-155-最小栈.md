## 最小栈

```python
class MinStack:

    '''
        用一个stack存放2元tuple元素(val, 当前 前缀数组中的最小值)
        e.g.
        nums = [-2, 0, -3, -1, -5, 1]
        依次push到min_stack中的话
        min_stack中栈底到栈口存的依次是
        (-2,-2), (-0,-2), (-3,-3), (-1,-3), (-5,-5), (1,-5) 

        为了保证第一个元素push后最小值的位置是自己 要特殊处理一下
    '''

    def __init__(self):
        self.min_st = []


    def push(self, val: int) -> None:
        if len(self.min_st) == 0:
            self.min_st.append((val, val))
        else:
            #取出当前的min值
            cur_min = self.min_st[-1][1] # 栈口 元组的第二个位置 即代表着当前的min值
            self.min_st.append((val, min(val, cur_min)))

    def pop(self) -> None:
        self.min_st.pop()

    def top(self) -> int:
        return self.min_st[-1][0]

    def getMin(self) -> int:
        return self.min_st[-1][1]

# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(val)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.getMin()
```
