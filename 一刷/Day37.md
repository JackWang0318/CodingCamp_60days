## 1. 组合数：外层遍历物品，内层遍历背包  Combination不考虑顺序
- 核心思想：

组合数不考虑顺序，只关心物品的选择方式。

通过外层遍历物品，内层遍历背包，可以确保每个物品只在前面的物品之后使用，从而避免重复计算相同的组合。

- 为什么？

外层遍历物品：固定物品的顺序，比如先考虑硬币1，再考虑硬币2，最后考虑硬币5。

内层遍历背包：对于每个物品，更新所有可能的背包容量（金额）。

由于物品是按顺序遍历的，后面的物品只能在前面的物品之后使用，因此不会出现“2+1”和“1+2”这样的重复组合。

## 2. 排列数：外层遍历背包，内层遍历物品 Arrangement考虑顺序
- 核心思想：

排列数考虑顺序，不同的顺序被视为不同的方法。

通过外层遍历背包，内层遍历物品，可以确保每个金额的更新都考虑了所有可能的物品，从而计算所有排列。

- 为什么？

外层遍历背包：固定背包容量（金额），从小到大更新。

内层遍历物品：对于每个金额，尝试所有可能的物品。

由于每次更新金额时都考虑了所有物品，因此会计算所有可能的排列。


## 完全背包理论基础
- 2025.3.2: 就是内层遍历 的顺序从倒序变成了正序

一维dp数组的情况下：
遍历是正序 先遍历物品i还是背包容量j 都可以
```python
## 先遍历物品

def test_CompletePack():
    weight = [1, 3, 4]
    value = [15, 20, 30]
    bagWeight = 4
    dp = [0] * (bagWeight + 1)
    for i in range(len(weight)):  # 遍历物品
        for j in range(weight[i], bagWeight + 1):  # 正序 遍历背包容量
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i])
    print(dp[bagWeight])


## 先遍历背包

def test_CompletePack():
    weight = [1, 3, 4]
    value = [15, 20, 30]
    bagWeight = 4

    dp = [0] * (bagWeight + 1)

    for j in range(bagWeight + 1):  # 遍历背包容量
        for i in range(len(weight)):  # 正序 遍历物品
            if j - weight[i] >= 0:
                dp[j] = max(dp[j], dp[j - weight[i]] + value[i])

    print(dp[bagWeight])

```

## 零钱兑换II

装满j容量的背包 有dp[j]种方法

```python
#看到无限零钱个数就知道是完全背包，
#但本题不是纯完全背包了（求是否能装满背包），而是求装满背包有几种方法。
#这里在遍历顺序上可就有说法了。
#如果求组合数就是外层for循环遍历物品，内层for遍历背包。
#如果求排列数就是外层for遍历背包，内层for循环遍历物品。
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        dp = [0] * (amount + 1)
        dp[0] = 1
        for i in range(len(coins)):
            for j in range(coins[i], amount + 1):
                dp[j] += dp[j - coins[i]]
            print(dp)
        return dp[amount]
```


## 组合总和IV
背包和物品放内外循环的问题 还可以再想想！！！
```python
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        # target（背包）放在外循环，将nums（物品）放在内循环，内循环从前到后遍历
        dp = [0] * (target + 1)
        dp[0] = 1
        for j in range(1, target + 1):  # 遍历背包
            for i in range(len(nums)):  # 遍历物品
                if j - nums[i] >= 0:
                    dp[j] += dp[j - nums[i]]
            # print(dp)
        return dp[target]

```
