## 路径总和iii 前缀和 二叉树
- 调试版
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> int:
        '''
            使用前缀和方法统计路径和等于targetSum的路径数量
            添加了详细的调试打印信息
        '''
        res = 0
        cnt = defaultdict(int)
        cnt[0] = 1  # 初始前缀和为0的路径有1条（空路径）
        print(f"初始化: cnt = {dict(cnt)}")

        def dfs(node, prefix_sum: int, depth=0):
            if node is None:
                print("  "*depth + f"到达空节点，返回")
                return
            
            nonlocal res
            prefix_sum += node.val
            print("  "*depth + f"访问节点 {node.val}, 当前前缀和 = {prefix_sum}")
            
            # 查找是否有 prefix_sum - targetSum 的前缀和存在
            target = prefix_sum - targetSum
            print("  "*depth + f"查找 target = {prefix_sum} - {targetSum} = {target}")
            print("  "*depth + f"当前cnt状态: {dict(cnt)}")
            found = cnt.get(target, 0)
            res += found
            print("  "*depth + f"找到 {found} 条路径, 当前总路径数 = {res}")
            
            # 更新当前前缀和的计数
            cnt[prefix_sum] += 1
            print("  "*depth + f"更新 cnt[{prefix_sum}] = {cnt[prefix_sum]}")
            
            # 递归处理左右子树
            print("  "*depth + "进入左子树...")
            dfs(node.left, prefix_sum, depth+1)
            print("  "*depth + "进入右子树...")
            dfs(node.right, prefix_sum, depth+1)
            
            # 回溯，恢复状态
            cnt[prefix_sum] -= 1
            print("  "*depth + f"回溯: cnt[{prefix_sum}] = {cnt[prefix_sum]}")
            if cnt[prefix_sum] == 0:
                del cnt[prefix_sum]
            print("  "*depth + f"回溯后cnt状态: {dict(cnt)}")

        print("开始深度优先搜索...")
        dfs(root, 0)
        print(f"搜索结束，最终结果 = {res}")
        return res
```


```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> int:
        '''
            前缀和
        '''
        res = 0
        cnt = defaultdict(int)
        cnt[0] = 1 # 前缀和为0 的个数为1  ； 即s[0]=0 

        def dfs(node, prefix_sum: int):
            if node is None:
                return
            nonlocal res
            prefix_sum += node.val
            res += cnt.get(prefix_sum - targetSum,0)
            cnt[prefix_sum] = cnt.get(prefix_sum,0) + 1
            dfs(node.left, prefix_sum)
            dfs(node.right, prefix_sum)
            cnt[prefix_sum] -= 1
            # 完成本层dfs

        dfs(root, 0)
        return res
```
