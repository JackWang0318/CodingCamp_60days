## 课程表 拓扑排序 BFS
- 借助indegrees和adjency邻接表  构建有向图
```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        '''
            本质就是拓扑排序 先收集各个节点的入度 和依赖关系 构造有向图
            判断整个图 是否是有向无环图
        '''
        indegrees = [0 for _ in range(numCourses)] # 储存每个节点的入度
        adjacency = [[] for _ in range(numCourses)]  # 邻接表来构造有向图
        queue = deque()
        # Get the indegree and adjacency of every course.
        for cur, pre in prerequisites:
            indegrees[cur] += 1
            adjacency[pre].append(cur)
        for i in range(len(indegrees)):
            if indegrees[i] == 0:
                queue.append(i)
        # BFS 拓扑排序
        while queue:
            pre = queue.popleft()
            numCourses -= 1 # 说明已经处理了一门课
            for cur in adjacency[pre]:
                # 去掉pre指向cur这个箭头
                indegrees[cur] -= 1
                # 添加新的入度为零的节点入队
                if not indegrees[cur]:
                    queue.append(cur) 
        
        return numCourses == 0
```
