### 并查集理论基础
- 并查集主要有两个功能：
- 将两个元素添加到一个集合中。
- 判断两个元素在不在同一个集合
### 寻找存在的路径
- 就是判断source节点和destination节点是否在同一个集合
```python
class UnionFind:
    """
    并查集（Union-Find）的实现，用于解决图的连通性问题。
    """
    def __init__(self, size):
        """
        初始化并查集。
        :param size: 并查集中节点的数量
        """
        # 初始化每个节点的父节点为自己
        self.parent = list(range(size + 1))  # 节点编号从 1 开始，因此范围是 0 到 size
    
    def find(self, u):
        """
        查找节点 u 的根节点，并进行路径压缩。
        :param u: 要查找的节点
        :return: 节点 u 的根节点
        """
        if self.parent[u] != u:
            # 递归查找根节点，并将路径上的节点直接连接到根节点
            self.parent[u] = self.find(self.parent[u])
        return self.parent[u]
    
    def union(self, u, v):
        """
        合并两个节点所在的集合。
        :param u: 第一个节点
        :param v: 第二个节点
        """
        # 找到两个节点的根节点
        root_u = self.find(u)
        root_v = self.find(v)
        # 如果两个节点的根节点不同，则合并它们
        if root_u != root_v:
            self.parent[root_v] = root_u  # 将 v 的根节点连接到 u 的根节点上
    
    def is_same(self, u, v):
        """
        判断两个节点是否在同一个集合中。
        :param u: 第一个节点
        :param v: 第二个节点
        :return: 如果两个节点在同一个集合中，返回 True，否则返回 False
        """
        return self.find(u) == self.find(v)


def main():
    """
    主函数：读取输入数据，使用并查集解决连通性查询问题。
    """
    import sys
    input = sys.stdin.read  # 从标准输入读取数据
    data = input().split()  # 将输入数据按空格分隔成列表
    
    index = 0
    # 读取节点数 n 和边数 m
    n = int(data[index])
    index += 1
    m = int(data[index])
    index += 1
    
    # 初始化并查集
    uf = UnionFind(n)
    
    # 读取 m 条边，逐条合并
    for _ in range(m):
        s = int(data[index])  # 边的起点
        index += 1
        t = int(data[index])  # 边的终点
        index += 1
        uf.union(s, t)  # 合并 s 和 t 所在的集合
    
    # 读取要查询连通性的两个节点
    source = int(data[index])
    index += 1
    destination = int(data[index])
    
    # 判断 source 和 destination 是否在同一个集合中
    if uf.is_same(source, destination):
        print(1)  # 如果连通，输出 1
    else:
        print(0)  # 如果不连通，输出 0


if __name__ == "__main__":
    main()

```
