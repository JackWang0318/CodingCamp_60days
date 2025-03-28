## 和为K的子数组

```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        ans = s = 0  # ans 是最终结果，s 是前缀和
        cnt = defaultdict(int)  # 用来记录各个前缀和出现的次数
        cnt[0] = 1  # 初始化：前缀和为0出现1次（即空数组的情况）
        for x in nums:
            s += x  # 计算当前的前缀和
            ans += cnt.get(s-k,0)  # 如果有前缀和等于s-k，说明存在子数组和为k
            cnt[s] += 1  # 记录当前前缀和出现的次数
            
        return ans
```
