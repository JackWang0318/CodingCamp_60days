# 代码随想录
## 数组
### 《代码随想录》数组：二分查找 近期作业较多 但还是得开个好头！写作业去了
#### 高中技术学科就接触过的算法 但还是略显生疏
尤其需要注意 while循环的执行条件 left 和right 的比较条件，这取决于你对查找区间边界的定义 
``` python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums) - 1   # 这里第一次调试的时候忘记-1了！
        while left <= right:   # 注意此处的区间 等号的斟酌
            mid = (left + right) // 2
            if target > nums[mid]:
                left = mid + 1
            elif target < nums[mid]:
                right = mid - 1
            else:
                return mid
        return -1
```


### 《代码随想录》数组：移除元素
#### 双指针法
双指针法 往往是 快指针去探路，慢指针来更新return的内容，根据题意来判断是否需要新建数组内容。

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        slowIndex = 0
        for fastIndex in range(len(nums)):
            if nums[fastIndex] != val: # 快指针探路有信息，说明要让慢指针更新了
                nums[slowIndex] = nums[fastIndex]
                slowIndex += 1
        return slowIndex
```


