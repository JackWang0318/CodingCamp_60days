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

### 孤岛的总面积

- 本题有些许区别，在于那些贴着矩阵边的陆地不算为要求的，孤岛的定义为，坐标不在矩阵边缘的陆地。
![image](https://github.com/user-attachments/assets/94f277fe-1ed1-417d-9856-38adaad0d8c4)

```python
# DFS版
position = [[1, 0], [0, 1], [-1, 0], [0, -1]]
count = 0

def dfs(grid, x, y):
    global count
    grid[x][y] = 0
    count += 1
    for i, j in position:
        next_x = x + i
        next_y = y + j
        if next_x < 0 or next_y < 0 or next_x >= len(grid) or next_y >= len(grid[0]):
            continue
        if grid[next_x][next_y] == 1: 
            dfs(grid, next_x, next_y)
                
n, m = map(int, input().split())

# 邻接矩阵
grid = []
for i in range(n):
    grid.append(list(map(int, input().split())))

# 清除边界上的连通分量
for i in range(n):
    if grid[i][0] == 1: 
        dfs(grid, i, 0)
    if grid[i][m - 1] == 1: 
        dfs(grid, i, m - 1)

for j in range(m):
    if grid[0][j] == 1: 
        dfs(grid, 0, j)
    if grid[n - 1][j] == 1: 
        dfs(grid, n - 1, j)
    
count = 0 # 将count重置为0
# 统计内部所有剩余的连通分量
for i in range(n):
    for j in range(m):
        if grid[i][j] == 1:
            dfs(grid, i, j)
            
print(count)
```

### 沉没孤岛
- 跟上一题有一点类似，只不过需要先将边界连通量特殊标记（为了和孤岛进行区分并且方便后续的复原），然后再把孤岛改为0，最后复原边界连通量即可
![image](https://github.com/user-attachments/assets/6c2ee85e-26b3-4d9a-aa1a-a2453d382528)

```python
# DFS版
def dfs(grid, x, y):
    grid[x][y] = 2
    directions = [(-1, 0), (0, -1), (1, 0), (0, 1)]  # 四个方向
    for dx, dy in directions:
        nextx, nexty = x + dx, y + dy
        # 超过边界
        if nextx < 0 or nextx >= len(grid) or nexty < 0 or nexty >= len(grid[0]):
            continue
        # 不符合条件，不继续遍历
        if grid[nextx][nexty] == 0 or grid[nextx][nexty] == 2:
            continue
        dfs(grid, nextx, nexty)

def main():
    n, m = map(int, input().split())
    grid = [[int(x) for x in input().split()] for _ in range(n)]

    # 步骤一：
    # 从左侧边，和右侧边 向中间遍历
    for i in range(n):
        if grid[i][0] == 1:
            dfs(grid, i, 0)
        if grid[i][m - 1] == 1:
            dfs(grid, i, m - 1)

    # 从上边和下边 向中间遍历
    for j in range(m):
        if grid[0][j] == 1:
            dfs(grid, 0, j)
        if grid[n - 1][j] == 1:
            dfs(grid, n - 1, j)

    # 步骤二、步骤三
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 1:
                grid[i][j] = 0
            if grid[i][j] == 2:
                grid[i][j] = 1

    # 打印结果
    for row in grid:
        print(' '.join(map(str, row)))

if __name__ == "__main__":
    main()
```

### 水流问题
- 从两个边界 逆流而上
![image](https://github.com/user-attachments/assets/998f8dc8-5114-4056-b889-36595750676c)

```python
first = set()
second = set()
directions = [[-1, 0], [0, 1], [1, 0], [0, -1]]

def dfs(i, j, graph, visited, side):
    if visited[i][j]:
        return
    
    visited[i][j] = True
    side.add((i, j))
    
    for x, y in directions:
        new_x = i + x
        new_y = j + y
        if (
            0 <= new_x < len(graph)
            and 0 <= new_y < len(graph[0])
            and int(graph[new_x][new_y]) >= int(graph[i][j])
        ):
            dfs(new_x, new_y, graph, visited, side)

def main():
    global first
    global second
    
    N, M = map(int, input().strip().split())
    graph = []
    for _ in range(N):
        row = input().strip().split()
        graph.append(row)
    
    # 是否可到达第一边界
    visited = [[False] * M for _ in range(N)]
    for i in range(M):
        dfs(0, i, graph, visited, first)
    for i in range(N):
        dfs(i, 0, graph, visited, first)
    
    # 是否可到达第二边界
    visited = [[False] * M for _ in range(N)]
    for i in range(M):
        dfs(N - 1, i, graph, visited, second)
    for i in range(N):
        dfs(i, M - 1, graph, visited, second)

    # 可到达第一边界和第二边界
    res = first & second
    
    for x, y in res:
        print(f"{x} {y}")
    
if __name__ == "__main__":
    main()
```

### 建造最大面积岛屿
- 先用DFS/BFS方式遍历并标记各岛屿的编号及对应面积，然后遍历水的元素，依次统计每个水(如果变成陆地的话)和周围邻接岛屿加起来的面积，存留最大值
