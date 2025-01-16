## 最小生成树算法(无向图)

### Prim算法
- 任选一个顶点
- 找跟其相连的边(权重最小)，连接该点，
- 然后将已连接的点当做一个整体，重复以上步骤
![image](https://github.com/user-attachments/assets/ddb79787-ba07-4a0a-b8c0-d11f8a793bc8)

```python
def prim(v, e, edges): # 顶点个数 边个数 图信息
    import sys
    import heapq

    # 初始化邻接矩阵，所有值初始化为一个大值(10001)，表示无穷大
    grid = [[10001] * (v + 1) for _ in range(v + 1)]

    # 读取边的信息并填充邻接矩阵
    for edge in edges:
        x, y, k = edge
        grid[x][y] = k
        grid[y][x] = k

    # 所有节点到最小生成树的最小距离
    minDist = [10001] * (v + 1)

    # 记录节点是否在树里
    isInTree = [False] * (v + 1)

    # Prim算法主循环
    for i in range(1, v):
        cur = -1
        minVal = sys.maxsize

        # 选择距离生成树最近的节点
        for j in range(1, v + 1):
            if not isInTree[j] and minDist[j] < minVal:
                minVal = minDist[j]
                cur = j

        # 将最近的节点加入生成树
        isInTree[cur] = True

        # 更新非生成树节点到生成树的距离
        for j in range(1, v + 1):
            if not isInTree[j] and grid[cur][j] < minDist[j]:
                minDist[j] = grid[cur][j]

    # 统计结果
    result = sum(minDist[2:v+1])
    return result

if __name__ == "__main__":
    import sys
    input = sys.stdin.read
    data = input().split()
    
    v = int(data[0])
    e = int(data[1])
    
    edges = []
    index = 2
    for _ in range(e):
        x = int(data[index])
        y = int(data[index + 1])
        k = int(data[index + 2])
        edges.append((x, y, k))
        index += 3

    result = prim(v, e, edges)
    print(result)

```

### Kruskal 算法

- 将边按照权重从小到大排序，依次遍历，并将边的两个点加入生成树
- 遍历过程中如果有环形成，则不加入该边
![image](https://github.com/user-attachments/assets/de683b93-63e1-490c-83fa-f4c24b70561e)

```python
class Edge:
    def __init__(self, l, r, val):
        self.l = l
        self.r = r
        self.val = val

n = 10001
father = list(range(n))

def init():
    global father
    father = list(range(n))

def find(u):
    if u != father[u]:
        father[u] = find(father[u])
    return father[u]

def join(u, v):
    u = find(u)
    v = find(v)
    if u != v:
        father[v] = u

def kruskal(v, edges):
    edges.sort(key=lambda edge: edge.val)
    init()
    result_val = 0

    for edge in edges:
        x = find(edge.l)
        y = find(edge.r)
        if x != y:
            result_val += edge.val
            join(x, y)

    return result_val

if __name__ == "__main__":
    import sys
    input = sys.stdin.read
    data = input().split()

    v = int(data[0])
    e = int(data[1])

    edges = []
    index = 2
    for _ in range(e):
        v1 = int(data[index])
        v2 = int(data[index + 1])
        val = int(data[index + 2])
        edges.append(Edge(v1, v2, val))
        index += 3

    result_val = kruskal(v, edges)
    print(result_val)
 
```
