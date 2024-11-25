## 分发饼干
大饼干喂饱大胃口 小饼干喂小胃口
注意先给胃口和饼干数组排序
然后是胃口作为for遍历，饼干是动态调整指针
```python
class Solution:
    def findContentChildren(self, g, s):
        # 这里的局部最优就是大饼干喂给胃口大的，充分利用饼干尺寸喂饱一个，
        # 全局最优就是喂饱尽可能多的小孩。
        g.sort()  # 将孩子的贪心因子排序
        s.sort()  # 将饼干的尺寸排序
        index = len(s) - 1  # 饼干数组的下标，从最后一个饼干开始
        result = 0  # 满足孩子的数量
        for i in range(len(g)-1, -1, -1):  # 遍历胃口，从最后一个孩子开始
            if index >= 0 and s[index] >= g[i]:  # 遍历饼干
                result += 1
                index -= 1
        return result

```

## 摆动序列 （求峰谷交替次数 最大值）
注意 平坡的情况处理 
峰谷的判断 

局部最优：删除单调坡度上的节点（不包括单调坡度两端的节点），那么这个坡度就可以有两个局部峰值。
整体最优：整个序列有最多的局部峰值，从而达到最长摆动序列。

```python
class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        if len(nums) <= 1:
            return len(nums)  # 如果数组长度为0或1，则返回数组长度
        preDiff,curDiff ,result  = 0,0,1  #题目里nums长度大于等于1，当长度为1时，其实到不了for循环里去，所以不用考虑nums长度
        for i in range(len(nums) - 1):
            curDiff = nums[i + 1] - nums[i]
            if curDiff * preDiff <= 0 and curDiff !=0:  #差值为0时，不算摆动
                result += 1
                preDiff = curDiff  #如果当前差值和上一个差值为一正一负时，才需要用当前差值替代上一个差值
        return result

```

## 最大子数组和
贪心的思路为局部最优：当前“连续和”为负数的时候立刻放弃，从下一个元素重新计算“连续和”，因为负数加上下一个元素 “连续和”只会越来越小。从而推出全局最优：选取最大“连续和”
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return nums[0]
        sum = 0
        max_sum = float('-inf')  # 初始化结果为负无穷大
        for i in range(len(nums)):
            sum += nums[i]
            if sum > max_sum:
                max_sum = sum
            if sum < 0:
                sum = 0
        return max_sum
    
```
