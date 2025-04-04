## 除法求值
- 构建双向有权图 + DFS
```python
class Solution:
    def calcEquation(
        self, equations: List[List[str]], values: List[float], queries: List[List[str]]
    ) -> List[float]:
        # create 有权图

        graph = {}
        for (start, end), val in zip(equations, values):
            if start not in graph:
                graph[start] = {}
            if end not in graph:
                graph[end] = {}
            graph[start][end] = val
            graph[end][start] = 1 / val

        def dfs(cur, tgt, acc, visited):
            """
            查找当前计算图中 cur作为除数 tgt作为被除数的 商
            acc: 传递累积值
            visited: 存储图中访问节点
            return 商 不存在则 返回-1
            """

            if cur == tgt:
                return acc
            if cur in visited:  # 说明存在环路
                return -1

            visited.add(cur)

            for e, v in graph[cur].items():
                found = dfs(e, tgt, acc * v, visited)
                if found != -1:
                    return found
            # 运行到这里说明没找到found!=-1情况
            return -1

        res = []
        for s, e in queries:
            if s not in graph or e not in graph:
                res.append(-1)
            else:
                res.append(dfs(s, e, 1.0, set()))
        return res

```
