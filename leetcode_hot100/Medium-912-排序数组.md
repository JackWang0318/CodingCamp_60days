## 快速排序

```python
# 基础版本
class Solution:
    def sortArray(self, nums: List[int]) -> List[int]:
        def partition(arr, low, high):
            pivot_idx = random.randint(low, high)                   # 随机选择pivot
            arr[low], arr[pivot_idx] = arr[pivot_idx], arr[low]     # pivot放置到最左边
            pivot = arr[low]                                        # 选取最左边为pivot
            # 以上都是选择pivot的策略
            # 此时low 指向的位置为'空' 因为已经被pivot指针提取出来了
            left, right =  low, high
            while left < right:
                while left < right and arr[right] >= pivot:         # 找到右边第一个<pivot的元素
                    right -= 1
                arr[left] = arr[right]                              # 并将其移动到left处
                
                while left < right and arr[left] <= pivot:          # 找到左边第一个>pivot的元素
                    left += 1
                arr[right] = arr[left]                              # 并将其移动到right处
            
            arr[left] = pivot                       # pivot放置到中间left=right处
            return left

        def quick_sort(arr, low, high):
            if low >= high:             # 递归结束
                return
            pivot_pos = partition(arr, low, high)
            quick_sort(arr, low, pivot_pos-1)             # 递归对pivot_pos两侧元素进行排序
            quick_sort(arr, pivot_pos+1, high)
        
        
        quick_sort(nums, 0, len(nums)-1)         # 调用快排函数对nums进行排序
        return nums



```


```python
# 三路快排

import random
from typing import List
class Solution:
    def sortArray(self, nums: List[int]) -> List[int]:
        def quicksort_3way(arr, low, high):
            if low >= high:
                return
            pivot_index = random.randint(low, high)
            arr[low], arr[pivot_index] = arr[pivot_index], arr[low]
            pivot = arr[low]

            lt = low        # arr[low+1 .. lt] < pivot
            gt = high       # arr[gt .. high] > pivot
            i = low + 1     # arr[lt+1 .. i] == pivot (未处理区间)

            while i <= gt:
                if arr[i] < pivot:
                    arr[i], arr[lt + 1] = arr[lt + 1], arr[i]
                    lt += 1
                    i += 1
                elif arr[i] > pivot:
                    arr[i], arr[gt] = arr[gt], arr[i]
                    gt -= 1
                else:
                    i += 1

            # 最后把 pivot 放到它该在的位置
            arr[low], arr[lt] = arr[lt], arr[low]

            # 递归处理左右两边
            quicksort_3way(arr, low, lt - 1)
            quicksort_3way(arr, gt + 1, high)

        quicksort_3way(nums, 0, len(nums) - 1)
        return nums

```
