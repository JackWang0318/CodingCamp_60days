## 背包问题 (二维数组)
![image](https://github.com/user-attachments/assets/3cc9291f-994b-4c93-ab64-9ad8f297ca7e)

- 1. 确定dp数组的下标和数组本身的含义
dp[i][j] 表示从下标为[0-i]的物品里任意取，放进容量为j的背包，价值总和最大是多少
- 2. 确定递推公式
 ![image](https://github.com/user-attachments/assets/15e90c05-d176-4145-a3d1-23841f7a292a)
![image](https://github.com/user-attachments/assets/051e1a33-18e1-4213-9cb0-e439dd7a477f)

递归公式： dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);
- 3. dp数组初始化
 ![image](https://github.com/user-attachments/assets/784b673a-8fde-4134-8647-12952589e700)

- 4. 确定遍历顺序
先遍历i或j都可以，偏向于先遍历物品i

- 5. 手动推到dp数组
 
```python
n, bagweight = map(int, input().split())

weight = list(map(int, input().split()))
value = list(map(int, input().split()))

dp = [[0] * (bagweight + 1) for _ in range(n)]

for j in range(weight[0], bagweight + 1):
    dp[0][j] = value[0]

for i in range(1, n):
    for j in range(bagweight + 1):
        if j < weight[i]:
            dp[i][j] = dp[i - 1][j]
        else:
            dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i])

print(dp[n - 1][bagweight])

```

## 背包问题 (一维数组) 状态压缩
因为当前行的计算是只依靠于上一行的数据， 所以可以每次把上一行的数据拷贝给当前行 然后直接在当前行计算，相当于把一个矩阵压缩成一行
j为背包容量
- 递推公式：一维dp数组，其实就上上一层 dp[i-1] 这一层 拷贝的 dp[i]来。

所以在 上面递推公式的基础上，去掉i这个维度就好。

递推公式为：dp[j] = max(dp[j], dp[j - weight[i]] + value[i])

- 初始化

- 遍历顺序：
先遍历物品，再倒序遍历背包容量

```python
n, bagweight = map(int, input().split())
weight = list(map(int, input().split()))
value = list(map(int, input().split()))

dp = [0] * (bagweight + 1)  # 创建一个动态规划数组dp，初始值为0

dp[0] = 0  # 初始化dp[0] = 0,背包容量为0，价值最大为0

for i in range(n):  # 应该先遍历物品，如果遍历背包容量放在上一层，那么每个dp[j]就只会放入一个物品
    for j in range(bagweight, weight[i]-1, -1):  # 倒序遍历背包容量是为了保证物品i只被放入一次
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i])

print(dp[bagweight])

```

## 分割等和子集

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        # 抽象为背包问题
        # 注意此处是严格要分为两个互补的子集 所有元素都要用到的
        if sum(nums) % 2 != 0:
            return False

        target = sum(nums) // 2
        dp = [0] * (target+1) # 数组下标为0~target 共 target+1个元素
        print(dp)
        for num in nums:
            for j in range(target, num-1, -1):
                dp[j] = max(dp[j], dp[j-num] + num)
        return dp[target] == target
```
