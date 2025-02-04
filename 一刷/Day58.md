### 拓扑排序
- 就是先找入度为零的节点 加入队列，然后重复步骤
- 那如果上面的依赖关系是一百对呢，一千对甚至上万个依赖关系，这些依赖关系中可能还有循环依赖，你如何发现循环依赖呢，又如果排出线性顺序呢？所以 拓扑排序就是专门解决这类问题的。概括来说，给出一个 有向图，把这个有向图转成线性的排序 就叫拓扑排序。当然拓扑排序也要检测这个有向图 是否有环，即存在循环依赖的情况，因为这种情况是不能做线性排序的。所以拓扑排序也是图论中判断有向无环图的常用方法。
![image](https://github.com/user-attachments/assets/84ad73d9-6389-477b-a4f1-09909aa09578)
- 那么 节点0 作为出发点 所连接的节点的入度 就都做了减一的操作。此时 节点1 和 节点 2 的入度都为0， 这样才能作为下一轮选取的节点。所以，我们在代码实现的过程中，本质是要将 该节点作为出发点所连接的节点的 入度 减一 就可以了，这样好能根据入度找下一个节点，不用真在图里把这个节点删掉。
```python
from collections import deque, defaultdict

def topological_sort(n, edges):
    inDegree = [0] * n # inDegree 记录每个文件的入度
    umap = defaultdict(list) # 记录文件依赖关系

    # 构建图和入度表
    for s, t in edges:
        inDegree[t] += 1
        umap[s].append(t)

    # 初始化队列，加入所有入度为0的节点
    queue = deque([i for i in range(n) if inDegree[i] == 0])
    result = []

    while queue:
        cur = queue.popleft()  # 当前选中的文件
        result.append(cur)
        for file in umap[cur]:  # 获取该文件指向的文件
            inDegree[file] -= 1  # cur的指向的文件入度-1
            if inDegree[file] == 0:
                queue.append(file)

    if len(result) == n:
        print(" ".join(map(str, result)))
    else:
        print(-1)


if __name__ == "__main__":
    n, m = map(int, input().split())
    edges = [tuple(map(int, input().split())) for _ in range(m)]
    topological_sort(n, edges)
```

### dijkstra朴素 算法 有向图 计算节点到节点最小距离
- 单源(到所有节点的最短路径) 有向图 无负数权重边
- ![image](https://github.com/user-attachments/assets/abefed0e-b747-4040-9321-3b0c232c2750)
```python
import sys

def dijkstra(n, m, edges, start, end):
    # 初始化邻接矩阵
    grid = [[float('inf')] * (n + 1) for _ in range(n + 1)]
    for p1, p2, val in edges:
        grid[p1][p2] = val

    # 初始化距离数组和访问数组
    minDist = [float('inf')] * (n + 1)
    visited = [False] * (n + 1)

    minDist[start] = 0  # 起始点到自身的距离为0

    for _ in range(1, n + 1):  # 遍历所有节点
        minVal = float('inf')
        cur = -1

        # 选择距离源点最近且未访问过的节点
        for v in range(1, n + 1):
            if not visited[v] and minDist[v] < minVal:
                minVal = minDist[v]
                cur = v

        if cur == -1:  # 如果找不到未访问过的节点，提前结束
            break

        visited[cur] = True  # 标记该节点已被访问

        # 更新未访问节点到源点的距离
        for v in range(1, n + 1):
            if not visited[v] and grid[cur][v] != float('inf') and minDist[cur] + grid[cur][v] < minDist[v]:
                minDist[v] = minDist[cur] + grid[cur][v]

    return -1 if minDist[end] == float('inf') else minDist[end]

if __name__ == "__main__":
    input = sys.stdin.read
    data = input().split()
    n, m = int(data[0]), int(data[1])
    edges = []
    index = 2
    for _ in range(m):
        p1 = int(data[index])
        p2 = int(data[index + 1])
        val = int(data[index + 2])
        edges.append((p1, p2, val))
        index += 3
    start = 1  # 起点
    end = n    # 终点

    result = dijkstra(n, m, edges, start, end)
    print(result)

``` 


### 堆优化版 dijkstra 
- 适合稀疏图, 利用邻接表的方式存储图
- 那么当从 边 的角度出发， 在处理 三部曲里的第一步（选源点到哪个节点近且该节点未被访问过）的时候 ，我们可以不用去遍历所有节点了。而且 直接把 边（带权值）加入到 小顶堆（利用堆来自动排序），那么每次我们从 堆顶里 取出 边 自然就是 距离源点最近的节点所在的边。这样我们就不需要两层for循环来寻找最近的节点了。

```python
import heapq

class Edge:
    def __init__(self, to, val):
        self.to = to
        self.val = val

def dijkstra(n, m, edges, start, end):
    grid = [[] for _ in range(n + 1)]

    for p1, p2, val in edges:
        grid[p1].append(Edge(p2, val))

    minDist = [float('inf')] * (n + 1)
    visited = [False] * (n + 1)

    pq = []
    heapq.heappush(pq, (0, start))
    minDist[start] = 0

    while pq:
        cur_dist, cur_node = heapq.heappop(pq)

        if visited[cur_node]:
            continue

        visited[cur_node] = True

        for edge in grid[cur_node]:
            if not visited[edge.to] and cur_dist + edge.val < minDist[edge.to]:
                minDist[edge.to] = cur_dist + edge.val
                heapq.heappush(pq, (minDist[edge.to], edge.to))

    return -1 if minDist[end] == float('inf') else minDist[end]

# 输入
n, m = map(int, input().split())
edges = [tuple(map(int, input().split())) for _ in range(m)]
start = 1  # 起点
end = n    # 终点

# 运行算法并输出结果
result = dijkstra(n, m, edges, start, end)
print(result)

```
  
