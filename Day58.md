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

### dijkstra 算法 有向图 计算节点到节点最小距离
