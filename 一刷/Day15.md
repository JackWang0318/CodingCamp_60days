## 平衡二叉树
- 2025.3.1:
**这里强调一波概念：
a. 二叉树节点的深度：指从根节点到该节点的最长简单路径边的条数。
b. 二叉树节点的高度：指从该节点到叶子节点的最长简单路径边的条数。**

**求二叉树节点深度 应该是前序(中左右) 因为是需要根节点的深度向下返回；同理，求高度就是后序(左右中)，是叶子节点的高度向上返回**


```python
class Solution:
    # 求深度差 即求高度差 此处是计算高度 所以是后序处理的逻辑 然后判断差值
    def getHeight(self,node) ->int:
        if not node: return 0 # 叶子结点
        # 左
        leftHeight = self.getHeight(node.left)
        if (leftHeight == -1): return -1
        rightHeight = self.getHeight(node.right)
        # 右
        if (rightHeight == -1): return -1
        # 中 
        if (abs(leftHeight - rightHeight) > 1):
            return -1
        else:
            return 1 + max(leftHeight, rightHeight)

    
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        if (self.getHeight(root) == -1):
            return False
        else:
            return True
```


## 二叉树的所有路径
### 有递归加回溯的思想 因为要遍历所有从根节点出发到达叶子结点的路径
- 2025.3.1
```python
class Solution:
    def traversal(self, cur, path, result):
        path.append(cur.val)  # 中
        print(f'## 遍历cur: {cur.val} path:{path}')
        if not cur.left and not cur.right:  # 到达叶子节点
            sPath = '->'.join(map(str, path))
            print(f'# 到叶节点' )
            result.append(Path)
            return
        if cur.left:  # 左孩子不为空
            self.traversal(cur.left, path, result)
            print(f'# 弹出 节点{path[-1]}')
            path.pop()  # 回溯
        if cur.right:  # 右不为空
            self.traversal(cur.right, path, result)
            print(f'# 弹出 节点{path[-1]}')
            path.pop()  # 回溯

    def binaryTreePaths(self, root):
        result = []
        path = []
        if not root:
            return result
        self.traversal(root, path, result)
        return result
```

## 左叶子之和

```python
class Solution:
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        # 平时我们解二叉树的题目时，已经习惯了通过节点的左右孩子判断本节点的属性，而本题我们要通过节点的父节点判断本节点的属性。
        if root is None: # 传入空节点 直接返回0
            return 0
        if root.left is None and root.right is None: #传入叶子结点 也返回0
            return 0
        
        leftValue = self.sumOfLeftLeaves(root.left)  # 左
        if root.left and not root.left.left and not root.left.right:  # 左子树是左叶子的情况
            # print(f'# 是左叶子 {root.left.val}')
            leftValue = root.left.val
            
        rightValue = self.sumOfLeftLeaves(root.right)  # 右

        sum_val = leftValue + rightValue  # 中， 要通过左右孩子传信息给父节点的思想 都是后序遍历
        return sum_val
```

## 计算完全二叉树的节点个数

```python
# 通法 适用于任何二叉树
class Solution:
    def countNodes(self, root: TreeNode) -> int:
        return self.getNodesNum(root)
        
    def getNodesNum(self, cur):
        if not cur:
            return 0
        leftNum = self.getNodesNum(cur.left) #左
        rightNum = self.getNodesNum(cur.right) #右
        treeNum = leftNum + rightNum + 1 #中
        return treeNum
# 方法2
# 借助完全二叉树的特点 如果是满二叉树 则直接用公式计算节点个数
# class Solution:
#     def countNodes(self, root: TreeNode) -> int:
#         if not root:
#             return 0
#         left = root.left
#         right = root.right
#         leftDepth = 0 #这里初始为0是有目的的，为了下面求指数方便
#         rightDepth = 0
#         while left: #求左子树深度
#             left = left.left
#             leftDepth += 1
#         while right: #求右子树深度
#             right = right.right
#             rightDepth += 1
#         if leftDepth == rightDepth:
#             return (2 << leftDepth) - 1 #注意(2<<1) 相当于2^2，所以leftDepth初始为0
#         return self.countNodes(root.left) + self.countNodes(root.right) + 1
```
