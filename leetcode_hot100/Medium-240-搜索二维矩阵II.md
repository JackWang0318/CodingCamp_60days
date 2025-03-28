## 搜索二维矩阵
![image](https://github.com/user-attachments/assets/1843381a-6cc9-4449-b0b8-0d2d566ff4d6)

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        '''
            根据矩阵的特殊分布进行筛选
        '''
        m, n = len(matrix), len(matrix[0])
        i, j = 0, n - 1  # 从右上角开始
        while i < m and j >= 0:  # 还有剩余元素
            if matrix[i][j] == target:
                return True  # 找到 target
            if matrix[i][j] < target:
                i += 1  # 这一行剩余元素全部小于 target，排除
            else:
                j -= 1  # 这一列剩余元素全部大于 target，排除
        return False

``
