
### 《代码随想录》数组：长度最小的子数组
#### 滑动窗口 
由于两年前“一刷”过，对滑动窗口方法有大致印象：大概就是左右边界随着某条件满足的前提下进行更新，找到right-left+1最小的状态。但是，写的时候还是对第一个循环的索引有疑惑，所以还是回去看了讲解：索引是滑动窗口的终止位置，即右边界。然后现在脑子里大概有那个滑动的动画过一遍了。
- 还有就是最后提交的时候发现由于给min_length初始化的地方和最后return条件的地方冲突了 导致部分测试用例失败了
```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        # 滑动窗口方法
        left = 0
        sum = 0
        min_length = len(nums) + 1  #初始化 其实可以设为Inf
        # 滑动窗口的右边界i.e.终止位置 作为for循环索引
        for right in range(len(nums)):
            sum += nums[right]
            # 满足sum>=target的条件下 去动态调整窗口的起始位置
            while (sum >= target):
                length = right - left + 1
                sum -= nums[left]
                left += 1
                if  length < min_length: min_length = length
            
        if min_length < len(nums) + 1:
            return min_length
        else:
            return 0
```


### 《代码随想录》数组：螺旋矩阵II
#### 螺旋矩阵，注意操作的区间开闭统一
很容易疏忽细节的内容，此处是四个边界的初始化以及每次沿着边界填充的区间开闭统一性。
```python
class Solution(object):
    def generateMatrix(self, n):
        if n <= 0:
            return []
        
        # 初始化 n x n 矩阵
        matrix = [[0]*n for _ in range(n)]

        # 初始化边界和起始值
        top, bottom, left, right = 0, n-1, 0, n-1
        num = 1

        while top <= bottom and left <= right:
            # 从左到右 填充 上边界
            for i in range(left, right + 1):
                matrix[top][i] = num
                num += 1
            # 压缩边界
            top += 1

            # 从上到下填充右边界
            for i in range(top, bottom + 1):
                matrix[i][right] = num
                num += 1
            # 压缩边界

            right -= 1

            # 从右到左填充下边界

            for i in range(right, left - 1, -1):
                matrix[bottom][i] = num
                num += 1
            # 压缩边界
            bottom -= 1

            # 从下到上填充左边界

            for i in range(bottom, top - 1, -1):
                matrix[i][left] = num
                num += 1
            # 压缩边界
            left += 1

        return matrix
```

