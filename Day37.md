## 完全背包理论基础
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
