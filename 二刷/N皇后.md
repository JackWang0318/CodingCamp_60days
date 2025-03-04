## N皇后 回溯问题 Hard
![image](https://github.com/user-attachments/assets/7b854fb5-fc55-4522-bac2-d14d4069fe5a)

```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        results = [] 
        chessboard = ['.' * n for _ in range(n)]
        self.backtracking(n, 0, chessboard, results)

        return [[''.join(row)for row in res] for res in results]

    def backtracking(self, n: int, row: int, chessboard: List[str], results: List[List[str]]) -> None:
        if row == n: # 终止条件
            results.append(chessboard[:])
            return
        
        for col in range(n):  # col维度是 回溯中的横向遍历
            if self.isValid(row, col, chessboard):
                chessboard[row] = chessboard[row][:col] + 'Q' + chessboard[row][col+1:] # 放置皇后
                self.backtracking(n, row + 1, chessboard, results) # 递归到下一行 row维度是 回溯中的纵向遍历
                chessboard[row] = chessboard[row][:col] + '.' + chessboard[row][col+1:]  # 回溯，撤销当前位置的皇后， 是为了探索每一种可能性 说白了跟暴力枚举没区别
         
    def isValid(self, row: int, col: int, chessboard: List[str]) -> bool:
        '''
            判断(row,col)坐标上放Queen的话 在棋盘chessboard这个二维数组中是否 合法
            Returns: bool
        '''
        # 分别检查列 交叉对角 是否有皇后
        for i in range(row):
            if chessboard[i][col] == 'Q':
                return False
        
        # 检查 45 度角是否有皇后
        i, j = row - 1, col - 1
        while i >= 0 and j >= 0:
            if chessboard[i][j] == 'Q':
                return False  # 左上方向已经存在皇后，不合法
            i -= 1
            j -= 1

        # 检查 135 度角是否有皇后
        i, j = row - 1, col + 1
        while i >= 0 and j < len(chessboard):
            if chessboard[i][j] == 'Q':
                return False  # 右上方向已经存在皇后，不合法
            i -= 1
            j += 1
        return True

```
