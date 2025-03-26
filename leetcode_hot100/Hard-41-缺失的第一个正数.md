## 原地哈希

```python
from typing import List
class Solution:

    # 3 应该放在索引为 2 的地方
    # 4 应该放在索引为 3 的地方

    def firstMissingPositive(self, nums: List[int]) -> int:
        '''
            将数组元素映射成
            nums[i]值对应i+1 的样子 
            eg:     nums:  1 2 3 4
                    index: 0 1 2 3           
        '''
        size = len(nums)
        for i in range(size):
            # 先判断nums[i]是否符合索引范围，然后判断nums[i]是不是放在了正确的索引处
            print(nums[i])
            while 1 <= nums[i] <= size and nums[i] != nums[nums[i] - 1]:
                self.__swap(nums, i, nums[i] - 1)
        # 按照索引从小到大扫描一遍
        for i in range(size):
            if i + 1 != nums[i]:
                return i + 1
        return size + 1

    def __swap(self, nums, index1, index2): #交换辅助函数 
        nums[index1], nums[index2] = nums[index2], nums[index1]

```
