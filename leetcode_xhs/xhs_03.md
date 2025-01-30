## 丑数 系列
- 链接：https://labuladong.online/algo/frequency-interview/ugly-number-summary/
![image](https://github.com/user-attachments/assets/ed9eb848-8787-418d-a6db-a5fc1f2ceb33)

```python
class Solution:
    def f(self, num: int, a: int, b: int, c: int) -> int:
        '''
            return: 在1-num的区间内 是a,b,c的丑数的个数
        '''
        return num // a + num // b + num // c - num // math.lcm(a, b) - num // math.lcm(a, c) - num // math.lcm(b, c) \
            + num // math.lcm(a, b, c)

    def nthUglyNumber(self, n: int, a: int, b: int, c: int) -> int:
        left, right = 1, int(2e9)
        while left <= right:
            mid = left + (right - left) // 2
            if self.f(mid, a, b, c) < n:
                left = mid + 1
            else:
                right = mid - 1
        return left

```
