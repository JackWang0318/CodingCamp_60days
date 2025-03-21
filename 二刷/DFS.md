## DFS

```python
# adj : List[List[int]] 邻接表
# vis : List[bool] 记录节点是否已经遍历

def dfs(s: int) -> None:
    stack = [s]  # 用列表来模拟栈，把起点加入栈中
    vis[s] = True  # 起点被遍历
    while stack:  # 当栈非空时继续执行
        u = (
            stack.pop()
        )  # 拿取并丢弃掉最后一个元素（栈顶的元素），可以理解为走到u这个元素
        for v in adj[u]:  # 对于与u相邻的每个元素v
            if not vis[v]:  # 如果v在此前没有走过
                vis[v] = True  # 确保栈里没有重复元素
                stack.append(v)  # 把v加入栈中
```

- 递归
```python
# adj : List[List[int]] 邻接表
# vis : List[bool] 记录节点是否已经遍历

def dfs(u: int) -> None:
    vis[u] = True
    for v in adj[u]:
        if not vis[v]:
            dfs(v)
```
