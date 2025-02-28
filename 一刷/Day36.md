## 最后一块石头的重量II
- 2025.2.28: 01背包 分割等和子集的等效变式
```python
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        # 思考一波 之后 发现其实就是 分割等和子集
        # 然后转化01背包
        total_sum = sum(stones)
        target = total_sum // 2
        dp = [0] * (target + 1)
        for stone in stones:
            for j in range(target, stone - 1, -1):
                dp[j] = max(dp[j], dp[j - stone] + stone)
        return total_sum - 2* dp[-1] # 只是最后的return 形式不同
```

## 目标和
- 2025.2.28: 感觉背包问题就主要需要厘清dp数组的下标及含义(一般就是题目直接问的，方案总数，True or False等)，还有什么是背包容量，什么是物品的价值，
```python
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:

        ## 抽象为背包问题，left - right = target; left + right = sum; target和sum已知的
        total_sum = sum(nums)  # 计算nums的总和
        if abs(target) > total_sum:
            return 0  # 此时没有方案
        if (target + total_sum) % 2 == 1:
            return 0  # 此时没有方案
        target_sum = (target + total_sum) // 2  # 目标和
        dp = [0] * (target_sum + 1)  # 创建动态规划数组，初始化为0
        dp[0] = 1  # 当目标和为0时，只有一种方案，即什么都不选
        for num in nums:
            for j in range(target_sum, num - 1, -1):
                dp[j] += dp[j - num]  # 状态转移方程，累加不同选择方式的数量 (不拿物品num和拿物品num的方法 之和)
        return dp[target_sum]  # 返回达到目标和的方案数

```

# 一和零

- 2025.2.28: 其实就是物品weight feature的维度增加了，就是每个物品(str)有两种重量维度(0的数量,1的数量)；dp[i][j]:装满i个0和j个1下 (i.e. 背包容量)，最多可以存放的字符串个数,最大子集(value)

```python
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        # dp[i][j]：最多有i个0和j个1的strs的最大子集的大小为dp[i][j]
        dp = [[0] * (n + 1) for _ in range(m + 1)]  # 创建二维动态规划数组，初始化为0
        # 遍历物品
        for s in strs:
            ones = s.count('1')  # 统计字符串中1的个数
            zeros = s.count('0')  # 统计字符串中0的个数
            # 遍历背包容量且从后向前遍历 (因为此处是状态压缩下的dp数组)
            # 这里遍历背包容量的两层for循环先后顺序没讲究，都是物品重量的一个维度，先遍历哪个都行！
            for i in range(m, zeros - 1, -1):
                for j in range(n, ones - 1, -1):
                    dp[i][j] = max(dp[i][j], dp[i - zeros][j - ones] + 1)  # 状态转移方程
        return dp[m][n]

```
