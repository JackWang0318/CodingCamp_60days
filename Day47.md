### 单调栈: 每日温度
![image](https://github.com/user-attachments/assets/19b4cdd1-a304-4295-b404-2134cf3c932c)


```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        answer = [0]*len(temperatures)
        stack = [0]  # 模拟栈 第一个是栈顶 最后一个是栈口 储存temperatures数组元素下标 
        for i in range(1,len(temperatures)):
            # 情况一和情况二
            if temperatures[i]<=temperatures[stack[-1]]:
                stack.append(i)
            # 情况三
            else:
                while len(stack) != 0 and temperatures[i]>temperatures[stack[-1]]:
                    answer[stack[-1]]=i-stack[-1] # 记录结果
                    stack.pop()
                stack.append(i)
        return answer
```

### 下一个更大元素I
- 注意nums1是nums2的子集 所以就直接处理Nums2然后再映射到nums1
```python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        result = [-1]*len(nums1)
        stack = [0]   #储存的是nums2的下标 所以初始是0
        for i in range(1,len(nums2)): # 所以这里range是从1开始
            # 情况一情况二
            if nums2[i]<=nums2[stack[-1]]:
                stack.append(i)
            # 情况三
            else:
                while len(stack)!=0 and nums2[i]>nums2[stack[-1]]: # 持续比较
                    if nums2[stack[-1]] in nums1:  # 找nums1中对应下标 收获结果
                        index = nums1.index(nums2[stack[-1]])
                        result[index]=nums2[i]
                    stack.pop() #弹出栈顶元素                  
                stack.append(i)
        return result
```

### 下一个更大元素II
- 循环数组 用mod
```python
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        result = [-1]*len(nums)
        stack = [0]   #储存的是nums2的下标 所以初始是0
        for i in range(1,len(nums)*2): # 所以这里range是从1开始
            # 情况一情况二
            if i >= len(nums):
                i = i % (len(nums))
            if nums[i]<=nums[stack[-1]]:
                stack.append(i)
            # 情况三
            else:
                while len(stack)!=0 and nums[i]>nums[stack[-1]]: # 持续比较
                    result[stack[-1]]=nums[i]
                    stack.pop() #弹出栈顶元素                  
                stack.append(i)
        return result
```
