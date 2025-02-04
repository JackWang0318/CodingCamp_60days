### 110.字符串接龙 
- 这里无向图求最短路，广搜最为合适，广搜只要搜到了终点，那么一定是最短的路径。因为广搜就是以起点中心向四周扩散的搜索。
- 本题是一个无向图，需要用标记位，标记着节点是否走过，否则就会死循环！使用set来检查字符串是否出现在字符串集合里更快一些
![image](https://github.com/user-attachments/assets/4051fa10-1521-45cd-bd0a-4d71efd7504f)

```python
def judge(s1, s2):
    """
    判断两个字符串是否仅有一个字符不同。
    :param s1: 字符串1
    :param s2: 字符串2
    :return: 如果两个字符串仅有一个字符不同，返回 True，否则返回 False
    """
    count = 0  # 记录不同字符的个数
    for i in range(len(s1)):  # 遍历字符串的每个字符
        if s1[i] != s2[i]:  # 如果对应位置字符不同
            count += 1
    return count == 1  # 如果仅有一个字符不同，返回 True

if __name__ == '__main__':
    # 输入 n 表示字符串列表的长度
    n = int(input())
    # 输入初始字符串和目标字符串
    beginstr, endstr = map(str, input().split())
    
    # 如果初始字符串和目标字符串相同，直接输出 0 并退出
    if beginstr == endstr:
        print(0)
        exit()
    # 输入 n 个字符串到列表 strlist 中
    strlist = []
    for i in range(n):
        strlist.append(input())
    # 使用 BFS（广度优先搜索）来寻找最短路径
    # 初始化访问标记列表，表示哪些字符串已经被访问过
    visit = [False for i in range(n)]
    # 初始化队列，存储当前的字符串和对应的步数，开始时步数为 1
    queue = [[beginstr, 1]]
    
    while queue:
        # 从队列中取出一个节点（当前字符串和步数）
        str, step = queue.pop(0)
        # 如果当前字符串与目标字符串仅有一个字符不同，则找到路径，输出步数并退出
        if judge(str, endstr):
            print(step + 1)
            exit()
        # 遍历字符串列表中的每一个字符串
        for i in range(n):
            # 如果该字符串未被访问过，且与当前字符串仅有一个字符不同
            if not visit[i] and judge(strlist[i], str):
                # 标记为已访问
                visit[i] = True
                # 将该字符串和更新后的步数加入队列
                queue.append([strlist[i], step + 1])
    # 如果 BFS 搜索完成后仍未找到路径，输出 0
    print(0)

```

### 105.有向图的完全可达性 
- 这里的深度搜索不需要回溯，因为不需要获取路径，只需要判断是否连通即可。
```python
def dfs(graph, key, visited):
    """
    深度优先搜索 (DFS) 算法，用于遍历图的节点。
    :param graph: 图的邻接表表示
    :param key: 当前节点
    :param visited: 已访问节点的标记数组
    """
    for neighbor in graph[key]:  # 遍历当前节点的所有邻居
        if not visited[neighbor]:  # 如果邻居节点未被访问
            visited[neighbor] = True  # 标记该邻居节点为已访问
            dfs(graph, neighbor, visited)  # 递归访问该邻居节点

def main():
    """
    主函数：读取输入数据，构建图并判断图的连通性。
    """
    import sys
    input = sys.stdin.read  # 从标准输入读取数据
    data = input().split()  # 将数据按空格分隔成列表

    # 读取节点数 n 和边数 m
    n = int(data[0])
    m = int(data[1])
    
    # 初始化图的邻接表表示，graph[i] 存储节点 i 的所有邻居
    graph = [[] for _ in range(n + 1)]
    
    # 读取 m 条边，并构建图的邻接表
    index = 2  # 边信息的起始索引
    for _ in range(m):
        s = int(data[index])  # 边的起点
        t = int(data[index + 1])  # 边的终点
        graph[s].append(t)  # 添加从 s 到 t 的边
        index += 2  # 更新索引，读取下一条边

    # 初始化访问标记数组，大小为 n + 1（因为节点编号从 1 开始）
    visited = [False] * (n + 1)
    
    # 从节点 1 开始遍历，标记为已访问
    visited[1] = True
    dfs(graph, 1, visited)  # 深度优先搜索，从节点 1 开始

    # 检查是否所有节点都被访问
    for i in range(1, n + 1):
        if not visited[i]:  # 如果有节点未被访问
            print(-1)  # 输出 -1 表示图不连通
            return
    
    # 如果所有节点都被访问，输出 1 表示图是连通的
    print(1)


if __name__ == "__main__":
    main()

```

### 106.岛屿的周长 
- 本题不需要DFS或者BFS，避免惯性思维！
- 计算出总的岛屿数量，总的变数为：岛屿数量 * 4; 因为有一对相邻两个陆地，边的总数就要减2，如图红线部分，有两个陆地相邻，总边数就要减2; 那么只需要在计算出相邻岛屿的数量就可以了，相邻岛屿数量为cover。
- 结果 result = 岛屿数量 * 4 - cover * 2;
![image](https://github.com/user-attachments/assets/d9b98d06-3a37-4bbb-8c61-4e417abdca33)


```python

def main():
    import sys
    input = sys.stdin.read
    data = input().split()
    
    # 读取 n 和 m
    n = int(data[0])
    m = int(data[1])
    
    # 初始化 grid
    grid = []
    index = 2
    for i in range(n):
        grid.append([int(data[index + j]) for j in range(m)])
        index += m
    
    sum_land = 0    # 陆地数量
    cover = 0       # 相邻数量

    for i in range(n):
        for j in range(m):
            if grid[i][j] == 1:
                sum_land += 1
                # 统计上边相邻陆地
                if i - 1 >= 0 and grid[i - 1][j] == 1:
                    cover += 1
                # 统计左边相邻陆地
                if j - 1 >= 0 and grid[i][j - 1] == 1:
                    cover += 1
                # 不统计下边和右边，避免重复计算
    
    result = sum_land * 4 - cover * 2
    print(result)

if __name__ == "__main__":
    main()

```
