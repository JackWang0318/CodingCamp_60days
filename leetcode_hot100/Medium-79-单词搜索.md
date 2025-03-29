## 单词搜索 DFS
![image](https://github.com/user-attachments/assets/7fd9aa63-821f-4d9d-9817-6674a021970d)

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        def dfs(i,j,k):
            '''
                i,j: 元素索引
                k: 已匹配的长度
            '''
            # 索引越界或元素不匹配
            if not 0 <= i < len(board) or not 0 <= j < len(board[0]) or board[i][j] != word[k]: return False
            # 字符串匹配完成
            if k == len(word) - 1: return True
            # 下面的处理逻辑是当前元素符合 但整个字符串还没匹配完成：

            # 标记本索引处元素已访问
            board[i][j] = ''
            # 返回上下左右四个相邻元素的结果
            res = dfs(i + 1, j, k + 1) or dfs(i - 1, j, k + 1) or dfs(i, j + 1, k + 1) or dfs(i, j - 1, k + 1)
            # 回溯
            board[i][j] = word[k]
            return res
        for i in range(len(board)):
            for j in range(len(board[0])):
                if dfs(i, j, 0): return True
        return False

```
