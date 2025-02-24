## 岛屿数量 
- DFS 解法

```python
class Solution:
    def dfs(self, grid, visited, x, y):
        '''
        深度优先搜索算法
        Params:
        grid: 2-D list 
        visited: 2-D list
        x,y : coordinate indices

        To search the nearby '1's around (x,y) in the grid(param) and label all the nearby '1's in visite(param)

        Returns:
        No return object, directly modify visited(param)
        '''
        direction = [[0, 1], [1, 0], [0, -1], [-1, 0]]
        for i,j in direction:
            next_x = x + i
            next_y = y + j
            # 下标越界，跳过
            if next_x < 0 or next_x >= len(grid) or next_y < 0 or next_y >= len(grid[0]):
                continue
            # 未访问的陆地，标记并调用深度优先搜索
            if not visited[next_x][next_y] and grid[next_x][next_y] == '1':
                visited[next_x][next_y] = True
                self.dfs(grid, visited, next_x, next_y)


    def numIslands(self, grid: List[List[str]]) -> int:
        row = len(grid)
        col = len(grid[0])

        visited = [[False] * col for _ in range(row)]
        res = 0 # 记录岛屿个数
        for i in range(row):
            for j in range(col):
                if grid[i][j] == '1' and not visited[i][j]:
                    print(f'cur index:({i},{j})\n')
                    res += 1
                    visited[i][j] = True
                    self.dfs(grid, visited, i, j)
        return res
```
