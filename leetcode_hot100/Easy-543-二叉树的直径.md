## 二叉树的直径 与最大深度联系

![image](https://github.com/user-attachments/assets/e42d5849-cbb4-452b-913d-48a25c5bdc3e)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:

    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        '''
        联系之前求最大深度 的后序遍历 
        其实区别就在于用一个ans变量来存每个节点的左右子树最大深度之和 
        '''
        self.ans = 0
        
        def depth(root):
            if not root: return 0
            L = depth(root.left)
            R = depth(root.right)
            self.ans = max(self.ans, L + R )
            return max(L, R) + 1

        depth(root)
        return self.ans


```
