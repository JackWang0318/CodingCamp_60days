## 图论
采用ACM的格式 不是leetcode形式了
### 所有可达路径
```python
# 邻接矩阵写法 graph[x][i]表示 x->i 有无边
def dfs(graph, x, n, path, result):
    if x == n:
        result.append(path.copy())
        return
    for i in range(1, n + 1):
        if graph[x][i] == 1:
            path.append(i)
            dfs(graph, i, n, path, result)
            path.pop()

def main():
    n, m = map(int, input().split())
    graph = [[0] * (n + 1) for _ in range(n + 1)]

    for _ in range(m):
        s, t = map(int, input().split())
        graph[s][t] = 1

    result = []
    dfs(graph, 1, n, [1], result)

    if not result:
        print(-1)
    else:
        for path in result:
            print(' '.join(map(str, path)))

if __name__ == "__main__":
    main()
```

### DFS 岛屿的数量
```python
direction = [[0, 1], [1, 0], [0, -1], [-1, 0]]  # 四个方向：上、右、下、左


def dfs(grid, visited, x, y):
    """
    对一块陆地进行深度优先遍历并标记
    """
    for i, j in direction:
        next_x = x + i
        next_y = y + j
        # 下标越界，跳过
        if next_x < 0 or next_x >= len(grid) or next_y < 0 or next_y >= len(grid[0]):
            continue
        # 未访问的陆地，标记并调用深度优先搜索
        if not visited[next_x][next_y] and grid[next_x][next_y] == 1:
            visited[next_x][next_y] = True
            dfs(grid, visited, next_x, next_y)


if __name__ == '__main__':  
    # 版本一
    n, m = map(int, input().split())
    
    # 邻接矩阵
    grid = []
    for i in range(n):
        grid.append(list(map(int, input().split())))
    
    # 访问表
    visited = [[False] * m for _ in range(n)]
    
    res = 0
    for i in range(n):
        for j in range(m):
            # 判断：如果当前节点是陆地，res+1并标记访问该节点，使用深度搜索标记相邻陆地。
            if grid[i][j] == 1 and not visited[i][j]:
                res += 1
                visited[i][j] = True
                dfs(grid, visited, i, j)
    
    print(res)
```
### BFS 岛屿的数量
```python

from collections import deque
directions = [[0, 1], [1, 0], [0, -1], [-1, 0]]
def bfs(grid, visited, x, y):
    que = deque([])
    que.append([x,y])
    visited[x][y] = True
    while que:
        cur_x, cur_y = que.popleft()
        for i, j in directions:
            next_x = cur_x + i
            next_y = cur_y + j
            if next_y < 0 or next_x < 0 or next_x >= len(grid) or next_y >= len(grid[0]): # 下标越界了 忽略
                continue
            if not visited[next_x][next_y] and grid[next_x][next_y] == 1: # 遍历到之前未标记过的陆地
                visited[next_x][next_y] = True
                que.append([next_x, next_y])


def main():
    n, m = map(int, input().split())
    grid = []
    for i in range(n):
        grid.append(list(map(int, input().split())))
    visited = [[False] * m for _ in range(n)]
    res = 0
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 1 and not visited[i][j]:
                res += 1
                bfs(grid, visited, i, j)
    print(res)

if __name__ == "__main__":
    main()

```
