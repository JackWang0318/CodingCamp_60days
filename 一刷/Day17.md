## 最大二叉树 
```python
class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> Optional[TreeNode]:
        # construct a Tree with a list
        if len(nums) ==1:
            return TreeNode(nums[0])
        
        # 找到数组中最大的值和对应的下标
        maxValue = 0
        maxValueIndex = 0
        node = TreeNode(maxValue)
        for i in range(len(nums)):
            if nums[i] > maxValue:
                maxValue = nums[i]
                maxValueIndex = i
        node.val = maxValue
        # 最大值所在的下标左区间 构造左子树
        if maxValueIndex > 0:
            new_list = nums[:maxValueIndex]
            node.left = self.constructMaximumBinaryTree(new_list)
        # 最大值所在的下标右区间 构造右子树
        if maxValueIndex < len(nums) - 1:
            new_list = nums[maxValueIndex+1:]
            node.right = self.constructMaximumBinaryTree(new_list)
        return node
```

## 合并二叉树 

```python
class Solution:
    def mergeTrees(self, root1: TreeNode, root2: TreeNode) -> TreeNode:
        # 递归终止条件: 
        #  但凡有一个节点为空, 就立刻返回另外一个. 如果另外一个也为None就直接返回None. 
        if not root1: 
            return root2
        if not root2: 
            return root1
        # ⚠️ 上面的递归终止条件保证了代码执行到这里root1, root2都非空. 
        # preOrder traversal
        root1.val += root2.val # 中
        root1.left = self.mergeTrees(root1.left, root2.left) #左
        root1.right = self.mergeTrees(root1.right, root2.right) # 右
        
        return root1 # ⚠️ 注意: 本题我们重复使用了题目给出的节点而不是创建新节点. 节省时间, 空间. 
```

## 二叉搜索树中的搜索 

```python
class Solution:
    def searchBST(self, root: TreeNode, val: int) -> TreeNode:
        # 为什么要有返回值: 
        #   因为搜索到目标节点就要立即return，
        #   这样才是找到节点就返回（搜索某一条边），如果不加return，就是遍历整棵树了。

        if not root or root.val == val: 
            return root

        if root.val > val: # search left
            return self.searchBST(root.left, val)

        if root.val < val: # search right
            return self.searchBST(root.right, val)
```


## 验证二叉搜索树 
```python
class Solution:
    def __init__(self):
        self.vec = []


    # 要知道中序遍历下，输出的二叉搜索树节点的数值是有序序列。

    # 有了这个特性，验证二叉搜索树，就相当于变成了判断一个序列是不是递增的了。

    def traversal(self, node:TreeNode):
        if node is None:
            return
        # 中序遍历
        self.traversal(node.left)
        self.vec.append(node.val)  # 将二叉搜索树转换为有序数组
        self.traversal(node.right)

    def isValidBST(self, root):
        self.vec = []  # 清空数组
        self.traversal(root)
        for i in range(1, len(self.vec)):
            # 注意要小于等于，搜索树里不能有相同元素
            if self.vec[i] <= self.vec[i - 1]:
                return False
        return True
```
