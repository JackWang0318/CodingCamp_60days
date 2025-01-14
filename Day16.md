## 中序后序构造二叉树

### ATTENTION: 仅已知前序和后序是无法唯一确定二叉树的
```python
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        # 第一步: 特殊情况讨论: 树为空. (递归终止条件)
        if not postorder:
            return None

        # 第二步: 后序遍历的最后一个就是当前的中间节点.
        root_val = postorder[-1]
        root = TreeNode(root_val)
        # print(f'## 当前父节点为 {root_val}')
        # 第三步: 找切割点.
        separator_idx = inorder.index(root_val) # 找值为root_val的中序数组下标(默认每个节点的val唯一)


        # 第四步: 切割inorder数组. 得到inorder数组的左,右半边.
        inorder_left = inorder[:separator_idx]
        inorder_right = inorder[separator_idx + 1:]
        # print(f'# 切割中序数组为： {inorder_left} 和 {inorder_right}')

        # 第五步: 切割postorder数组. 得到postorder数组的左,右半边.
        # ⭐️ 重点1: 中序数组大小一定跟后序数组大小是相同的.
        postorder_left = postorder[:len(inorder_left)]
        postorder_right = postorder[len(inorder_left): len(postorder) - 1]
        # print(f'# 切割后序数组为： {postorder_left} 和 {postorder_right}')

        # 第六步: 递归
        root.left = self.buildTree(inorder_left, postorder_left)
        root.right = self.buildTree(inorder_right, postorder_right)
         # 第七步: 返回答案
        # print(f'### return {root}')
        
        return root
```

### 同理 前序和后序构造二叉树也很easy了，注意切割数组时长度的控制 但是自己写的时候还是有细节出错

```python
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if not preorder:
            return None
        
        root_val = preorder[0]
        root = TreeNode(root_val) 
        # @@ 初始化节点 遗漏了 而且 初始化的值也搞错了 随便写了一个0 应该是root_val!

        seperate_index = inorder.index(root_val)
        inorder_left = inorder[:seperate_index]
        inorder_right = inorder[seperate_index + 1:]
        preorder_left = preorder[1:1+len(inorder_left)]
        preorder_right = preorder[1+len(inorder_left):]

        root.left = self.buildTree(preorder_left, inorder_left)
        root.right = self.buildTree(preorder_right,inorder_right)

        return root
```

## 路径总和i和ii
### i的递归函数需要返回值，因为找到一个符合条件的结果就可以返回True了
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def traversal(self, cur: TreeNode, count: int) -> bool:
        if not cur.left and not cur.right and count == 0: # 遇到叶子节点，并且计数为0
            return True
        if not cur.left and not cur.right: # 如果 上面没有return True 且遇到叶子节点直接返回False
            return False
        
        if cur.left: # 左
            count -= cur.left.val
            if self.traversal(cur.left, count): # 递归，处理节点
                return True
            count += cur.left.val # 回溯，撤销处理结果
            
        if cur.right: # 右
            count -= cur.right.val
            if self.traversal(cur.right, count): # 递归，处理节点
                return True
            count += cur.right.val # 回溯，撤销处理结果
            
        return False
    
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if root is None:
            return False
        return self.traversal(root, targetSum - root.val)      
```

### 路径总和ii要遍历整个树，找到所有路径，所以递归函数不要返回值！
```python
class Solution:
    def __init__(self):
        self.result = []
        self.path = []

    def traversal(self, cur, count):
        if not cur.left and not cur.right and count == 0: # 遇到了叶子节点且找到了和为sum的路径
            self.result.append(self.path[:])
            # print(f'##成功 找到路径{self.path}')

            return

        if not cur.left and not cur.right: # 遇到叶子节点而没有找到合适的边，直接返回
            # print(f'##遇到叶子节点而没有找到符合条件的path，回溯至父节点')
            return

        if cur.left: # 左 （空节点不遍历）
            # print(f'##遍历左叶节点 {cur.left.val}')
            self.path.append(cur.left.val)
            count -= cur.left.val
            self.traversal(cur.left, count) # 递归
            count += cur.left.val # 回溯
            self.path.pop() # 回溯

        if cur.right: #  右 （空节点不遍历）
            # print(f'##遍历右叶节点 {cur.right.val}')
            self.path.append(cur.right.val) 
            count -= cur.right.val
            self.traversal(cur.right, count) # 递归
            count += cur.right.val # 回溯
            self.path.pop() # 回溯

        return

    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        self.result.clear()
        self.path.clear()
        if not root:
            return self.result
        self.path.append(root.val) # 把根节点放进路径
        self.traversal(root, targetSum - root.val)
        return self.result 
```

## FindBottomLeftValue 递归回溯法

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findBottomLeftValue(self, root: TreeNode) -> int:
        # 初始化
        self.max_depth = float('-inf')
        self.result = None
        self.traversal(root, 0)
        return self.result
    
    def traversal(self, node, depth):
        if not node.left and not node.right:
            # 当遇到叶子节点的时候，就需要统计一下最大的深度了，所以需要遇到叶子节点来更新最大深度。
            if depth > self.max_depth:
                self.max_depth = depth
                self.result = node.val
            return
        # 先向左遍历 找最深左叶子节点 
        if node.left:
            depth += 1
            self.traversal(node.left, depth)
            depth -= 1
        if node.right:
            depth += 1
            self.traversal(node.right, depth)
            depth -= 1

```
