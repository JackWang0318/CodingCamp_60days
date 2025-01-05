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

