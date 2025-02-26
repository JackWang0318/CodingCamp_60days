## 翻转二叉树
- 2025.2.25: 晚上十一点看 感觉巨困 主要前几天刚做过对称二叉树 就很无感
```python
# 前序遍历 
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        # 递归三部曲 
        # 返回类型 和输入参数
        # 终止条件
        # 单层递归处理逻辑
        if (not root):
            return root
        root.left, root.right = root.right, root.left
        self.invertTree(root.left)  # 注意此处的self 容易漏
        self.invertTree(root.right)

        return root

注意中序遍历的话：
        self.invertTree(root.left)
        root.left, root.right = root.right, root.left
        self.invertTree(root.left) # 此处还是left 因为 上一行交换过了

```

## 对称二叉树

```python
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        # 本质在比较两个左右子树能否相互翻转

        return self.compare(root.left, root.right)

    def compare(self, left, right) -> bool:
        if (left == None and right != None): return False
        elif (left != None and right == None): return False
        elif (left == None and right == None): return True
        elif (left.val != right.val): return False
        print(left.left,right.right)
        outside = self.compare(left.left, right.right) # 比较外侧节点
        print(left.right, right.left)
        inside = self.compare(left.right, right.left) # 比较内侧节点
        return outside and inside

        # 后序遍历
    
```


## 二叉树的最大深度
```python
class Solution:
    # 根节点的高度就是整个二叉树的最大深度
    # 从而从而转化通过后序遍历求根节点的高度
    def getHeight(self, root) -> int:
        # print(f'cur:{root}')
        if not root: return 0
        leftHeight = self.getHeight(root.left)
        # print(leftHeight)
        rightHeight = self.getHeight(root.right)
        # print(rightHeight)
        Height = 1 + max(leftHeight, rightHeight)
        return Height
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        return self.getHeight(root)
```

## 最小深度
- 2025.2.25: 要注意叶子节点的判定
```python
class Solution:

    # 等价于求根节点对每一个叶子节点的高度 取最小值
    def getHeight(self, node):
        if node is None:
            return 0
        leftHeight = self.getHeight(node.left)  # 左
        rightHeight = self.getHeight(node.right)  # 右
        
        # 当一个左子树为空，右不为空，这时并不是最低点
        if node.left is None and node.right is not None:
            return 1 + rightHeight
        
        # 当一个右子树为空，左不为空，这时并不是最低点
        if node.left is not None and node.right is None:
            return 1 + leftHeight
        
        result = 1 + min(leftHeight, rightHeight)
        return result

    def minDepth(self, root):
        return self.getHeight(root)
```
