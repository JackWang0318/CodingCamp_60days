## 小红书 from leetcode

### 数组中的第K个最大元素
- 最小堆

```python
import heapq

class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        min_heap = []
        # 遍历数组
        for num in nums:
            # 将当前元素加入堆
            heapq.heappush(min_heap, num)
            # 如果堆的大小超过 k，移除堆顶元素
            if len(min_heap) > k:
                heapq.heappop(min_heap)
        # 堆顶就是第 k 大的元素
        return min_heap[0]

```

- 快速排序
