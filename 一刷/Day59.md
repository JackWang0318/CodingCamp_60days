### Bellman_ford 
![image](https://github.com/user-attachments/assets/15a12ee4-6152-4407-8cfb-0445273dbbad)

- 也就是说，如果 通过 A 到 B 这条边可以获得更短的到达B节点的路径，即如果 minDist[B] > minDist[A] + value，那么我们就更新 minDist[B] = minDist[A] + value ，这个过程就叫做 “松弛” 。
- 
```python
def main():
    n, m = map(int, input().strip().split())
    edges = []
    for _ in range(m):
        src, dest, weight = map(int, input().strip().split())
        edges.append([src, dest, weight])
    
    minDist = [float("inf")] * (n + 1)
    minDist[1] = 0  # 起点处距离为0
    
    for i in range(1, n):
        updated = False
        for src, dest, weight in edges:
            if minDist[src] != float("inf") and minDist[src] + weight < minDist[dest]:
                minDist[dest] = minDist[src] + weight
                updated = True
        if not updated:  # 若边不再更新，即停止回圈
            break
    
    if minDist[-1] == float("inf"):  # 返还终点权重
        return "unconnected"
    return minDist[-1]
    
if __name__ == "__main__":
    print(main())
```
