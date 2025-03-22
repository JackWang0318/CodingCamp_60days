##
```python
class Solution:
    def find(self, node):
        '''
            并查集寻找node节点的根节点
        '''
        if node != self.father[node]:
            return self.find(self.father[node])
        else:
            return node

    def minimumCost(self, n: int, connections: List[List[int]]) -> int:
        '''
            n: node数量, 默认构造值为1-n的节点
            connections: 每个元素[i,j,k] 表示信息 i->j, cost=k
            1.将所有的边按照权重从小到大排序。
            2.取一条权重最小的边。
            3.使用并查集（union-find）数据结构来判断加入这条边后是否会形成环。
                若不会构成环，则将这条边加入最小生成树中。
                检查所有的结点是否已经全部联通，这一点可以通过目前已经加入的边的数量来判断。
                若全部联通，则结束算法；否则返回步骤 2.
        '''
        self.father = [i for i in range(n + 1)]  # 1-based index
        # 对edge 按照cost进行升序排序
        connections.sort(key= lambda connection: connection[2]) 
        res = 0
        edge_count = 0
        for a,b,cost in connections: # 遍历connections(已经按照边权重升序sorted过的)
            root_a = self.find(a)
            root_b = self.find(b)
            if root_a !=  root_b: #如果两个节点不在同一个集合中 将root_a标记为root_b的父节点
                self.father[root_b] = root_a
                res += cost
                edge_count += 1

            if edge_count == n - 1: # 连接n个节点需要的边的数量
                return res
        # for循环 结束还没return 说明没有符合条件
        return -1

```
