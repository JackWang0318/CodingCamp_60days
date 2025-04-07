## 搜索旋转数组
- 注意区间和循环不变量以及最后下标返回值
  
```python
class Solution:
    '''
    两次二分 
    第一次先找到旋转数组的最小值的index
    第二次比较target和nums[-1]大小后 在对应的区间内找target
    '''

    # 153. 寻找旋转排序数组中的最小值（返回的是下标）
    def findMin(self, nums: List[int]) -> int:
        left, right = 0, len(nums) - 2  # 闭区间 [0, n-2]
        while left <= right:  # 闭区间不为空
            mid = (left + right) // 2
            if nums[mid] < nums[-1]:
                right = mid -1
            else:
                left = mid + 1
        return left

    # 有序数组中找 target 的下标
    def lower_bound(self, nums: List[int], left: int, right: int, target: int) -> int:
        while left <= right:
            mid = (left + right) // 2
            # 循环不变量：
            # nums[right] > target
            # nums[left] <= target
            if nums[mid] > target:
                right = mid - 1  # 范围缩小到 [left, mid-1]
            else:
                left = mid + 1  # 范围缩小到 [mid+1, right]
        return right if nums[right] == target else -1

    def search(self, nums: List[int], target: int) -> int:
        index_min = self.findMin(nums)
        if target > nums[-1]:
            return self.lower_bound(nums, 0, index_min - 1, target)
        else:
            return self.lower_bound(nums, index_min, len(nums) - 1, target)

```
