## 581
<img width="564" alt="image" src="https://github.com/user-attachments/assets/7fd2992f-74f3-47a4-879d-4d01abb04826" />

```python
class Solution:
    def findUnsortedSubarray(self, nums: List[int]) -> int:
        '''
        如果进入了右段，就没有比最大值小的数，所以最后一个比最大值小的数就是中段的右边界，同理，
        如果进入左段，就不会出现比最小值更大的情况，所以最后一个出现就视为中段左边界
        '''
        n = len(nums)
        begin = 0
        end = -1
        max = nums[0]
        min = nums[n-1]
        # i 从左往右扫 j从右往左扫
        for i in range(n):
            if nums[i] >= max:
                max = nums[i]
            else:
                end = i

            j = n - i - 1
            if nums[j] <= min:
                min = nums[j]
            else:
                begin = j
            
        return end - begin + 1

```
