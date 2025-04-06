## 树形dp

```python
class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        ans = -inf # 因为节点有负数
        def dfs(node: Optional[TreeNode]) -> int:
            if node is None:
                return 0  # 没有节点，和为 0
            l_val = dfs(node.left)  # 左子树最大链和
            r_val = dfs(node.right)  # 右子树最大链和
            nonlocal ans
            ans = max(ans, l_val + r_val + node.val)  # 两条链拼成路径 左右子树最大链长之和＋本节点val
            return max(max(l_val, r_val) + node.val, 0)  # 当前子树最大链和（注意这里和 0 取最大值了 因为节点有负数）
        dfs(root)
        return ans

```
