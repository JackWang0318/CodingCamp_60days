### 《代码随想录》哈希表：四数相加II
#### hashtable -> defaultdict
```python
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        from collections import defaultdict
        # iterate nums1 and nums2 and put the sum value in a dict
        hashtable = defaultdict(int)
        for n1 in nums1:
            for n2 in nums2:
                if (n1+n2) in hashtable:
                    hashtable[n1+n2] += 1
                else:
                    hashtable[n1+n2] = 1
        count = 0
        for n3 in nums3:
            for n4 in nums4:
                if -(n3+n4) in hashtable:
                    count += hashtable[-n3-n4]
        return count
```

### 《代码随想录》哈希表：赎金信
```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        hash_a = dict()
        for s in ransomNote:
            if s in hash_a:
                hash_a[s] += 1
            else:
                hash_a[s] = 1
        for v in magazine:
            if v in hash_a and hash_a[v] > 0:
                hash_a[v] -= 1
        for k,v in hash_a.items():
            if v != 0: return False
        return True
```


### 《代码随想录》哈希表：三数之和
#### So sleepy in WongAvery Lib

![image.png](https://camo.githubusercontent.com/195c8b882887df8176a7fc49b069b8a2696f1f47c962a9ebdc3fe961db473503/68747470733a2f2f636f64652d7468696e6b696e672e63646e2e626365626f732e636f6d2f676966732f31352e2545342542382538392545362539352542302545342542392538422545352539322538432e676966)
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        result = []
        nums.sort()
        # double pointer method 
        # need to sort the list first 
        for i in range(len(nums)):
            # 如果第一个元素已经大于0，不需要进一步检查
            if nums[i] > 0:
                return result
            # 跳过相同的元素以避免重复
            if i > 0 and nums[i] == nums[i - 1]:
                continue
                
            left = i + 1
            right = len(nums) - 1
            
            while right > left:
                sum_ = nums[i] + nums[left] + nums[right]
                
                if sum_ < 0:
                    left += 1
                elif sum_ > 0:
                    right -= 1
                else:
                    result.append([nums[i], nums[left], nums[right]])
                    
                    # 跳过相同的元素以避免重复
                    while right > left and nums[right] == nums[right - 1]:
                        right -= 1
                    while right > left and nums[left] == nums[left + 1]:
                        left += 1
                        
                    right -= 1
                    left += 1
                    
        return result
```


### 《代码随想录》哈希表：四数之和
#### FourSum

```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        n = len(nums)
        result = []
        for i in range(n):
            if nums[i] > target and nums[i] > 0 and target > 0:# 剪枝（可省）
                break
            if i > 0 and nums[i] == nums[i-1]:# 去重
                continue
            for j in range(i+1, n):
                if nums[i] + nums[j] > target and target > 0: #剪枝（可省）
                    break
                if j > i+1 and nums[j] == nums[j-1]: # 去重
                    continue
                left, right = j+1, n-1
                while left < right:
                    s = nums[i] + nums[j] + nums[left] + nums[right]
                    if s == target:
                        result.append([nums[i], nums[j], nums[left], nums[right]])
                        # del same cases 
                        while left < right and nums[left] == nums[left+1]:
                            left += 1
                        while left < right and nums[right] == nums[right-1]:
                            right -= 1
                        left += 1
                        right -= 1
                    elif s < target:
                        left += 1
                    else:
                        right -= 1
        return result
```


