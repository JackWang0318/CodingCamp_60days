### Floyd 算法 多源最短路径
- 动机：计算图中每个点到其他每一个点的最短路径
- 依次将每个点作为中间节点进行处理，图是邻接矩阵的形式来记录
- 用两个二维数组分别记录 最短路径距离，和path矩阵(记录路径)
![image](https://github.com/user-attachments/assets/f0444c17-fcf5-4631-b16b-64134a0d29de)

```python
def floyd_warshall_with_path(graph):
    # Number of vertices in the graph
    n = len(graph)
    
    # Distance matrix initialization 
    dist = [[float('inf')] * n for _ in range(n)]
    # Predecessor matrix initialization 记录路径
    pred = [[-1] * n for _ in range(n)]
    
    # Initialize the distance and predecessor matrices
    for i in range(n):
        for j in range(n):
            if i == j:
                dist[i][j] = 0
            elif graph[i][j] != 0:
                dist[i][j] = graph[i][j]
                pred[i][j] = i  # i is the predecessor of j
            else:
                dist[i][j] = float('inf')
                pred[i][j] = -1  # No path
    
    # Floyd-Warshall algorithm
    for k in range(n): # k 就是记录本次遍历的中间点 
        for i in range(n): # row
            for j in range(n): # col
                if dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]
                    pred[i][j] = pred[k][j]  # Update predecessor
    
    return dist, pred

# Function to reconstruct the path from predecessor matrix
def reconstruct_path(pred, start, end):
    path = []
    if pred[start][end] == -1:
        return path  # No path exists
    path.append(end)
    while start != end:
        end = pred[start][end]
        path.append(end)
    path.reverse()
    return path

# Example usage
graph = [
    [0, 3, float('inf'), 7],
    [8, 0, 2, float('inf')],
    [5, float('inf'), 0, 1],
    [2, float('inf'), float('inf'), 0]
]

dist, pred = floyd_warshall_with_path(graph)

# Print shortest distances
print("Shortest distances:")
for row in dist:
    print(row)

# Print paths
print("\nPaths:")
for i in range(len(graph)):
    for j in range(len(graph)):
        if i != j:
            path = reconstruct_path(pred, i, j)
            if path:
                print(f"Path from {i} to {j}: {path}")
            else:
                print(f"No path from {i} to {j}")
```


### A* 算法
- https://www.redblobgames.com/pathfinding/a-star/introduction.html 这个博客无敌了 可视化巨牛
- 局限性：![image](https://github.com/user-attachments/assets/f0eb0d2a-df66-4d74-9716-95e7e844a72e)

- heuristics():启发式函数用于 计算 预估代价

```python
def heuristic(a, b): # 此处用曼哈顿距离来计算预估代价
    # Manhattan distance on a square grid
    return abs(a.x - b.x) + abs(a.y - b.y)

def a_star_search(graph, start, goal):
    frontier = PriorityQueue() # 优先队列
    frontier.put(start, 0)
    came_from = {}
    cost_so_far = {}
    came_from[start] = None
    cost_so_far[start] = 0
  
    while not frontier.empty():
      current = frontier.get()
      if current == goal:
        break
      for next in graph.neighbors(current): # 遍历当前节点的邻居 (网格图的话就是上下左右)
        new_cost = cost_so_far[current] + graph.cost(current, next)  # 计算新的 当前代价 = 之前到达current的代价 + 从current到next的代价 (网格图的话就是1)
        if next not in cost_so_far or new_cost < cost_so_far[next]:# 如果next节点没有遍历过 或者 new_cost小于之前算过的到next节点的代价， 就进行更新
          cost_so_far[next] = new_cost
          priority = new_cost + heuristics(start, goal) # 计算总代价
          frontier.put(next, priority)
          came_from[next] = current
    return came_from, cost_so_far


```
