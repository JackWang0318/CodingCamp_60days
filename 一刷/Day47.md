### 单调栈: 每日温度
![image](https://github.com/user-attachments/assets/19b4cdd1-a304-4295-b404-2134cf3c932c)

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:

        '''
            栈存的是下标 因为最后要求的是下标之间的差
            情况一：当前遍历的元素T[i]小于栈顶元素T[st.top()]的情况
            情况二：当前遍历的元素T[i]等于栈顶元素T[st.top()]的情况
            情况三：当前遍历的元素T[i]大于栈顶元素T[st.top()]的情况
        '''
        answer = [0]*len(temperatures)
        stack = [0]  # 模拟栈 第一个是栈底 最后一个是栈口 储存temperatures数组元素下标 
        for i in range(1,len(temperatures)):
            # 情况一和情况二
            if temperatures[i]<=temperatures[stack[-1]]:
                # print(f'当前遍历的元素T[i]:{temperatures[i]}<=栈顶元素T[stack[-1]]:{temperatures[stack[-1]]}')
                # print(f'栈压入下标{i},之后会处理')
                stack.append(i)
            # 情况三
            else:
                while len(stack) != 0 and temperatures[i]>temperatures[stack[-1]]:
                    # print(f'当前遍历的元素T[i]:{temperatures[i]}>栈顶元素T[stack[-1]]:{temperatures[stack[-1]]}')
                    # print(f'记录当前栈顶元素下标{i}的answer,并弹出栈')
                    answer[stack[-1]]=i-stack[-1] # 记录结果
                    stack.pop()
                # print(f'栈压入下标{i},之后会处理')
                stack.append(i)
            # print(f'stack: {stack}')
        return answer
```

### 下一个更大元素I
- 2025.3.27
- 注意nums1是nums2的子集 所以就直接处理nums2然后再映射到nums1的下标即可 (其实就是对nums2的每个元素找右边第一个比其大的值)
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
                    if nums2[stack[-1]] in nums1:
                        # 找nums1中对应下标 收获结果
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
