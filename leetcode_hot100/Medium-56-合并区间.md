## 合并区间
- 跟那个 根据身高重新建队列有点像 最好是一开始就先根据interval的左区间端点升序排序 方便之后的处理
```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort(key=lambda x: x[0]) # 按照左区间点 升序排列
        res = []
        for intv in intervals:
            if len(res) != 0 and res[-1][1] >= intv[0]: # 可以合并
                res[-1][1] = max(res[-1][1], intv[1])  # 右区间点 取max
            else: # 不可以合并 直接添加新区间
                res.append(intv)
        return res


```
