## 二叉树flatten
![image](https://github.com/user-attachments/assets/87c71d13-c69c-48e7-abfc-6bdef036c00f)

```python
# 前序遍历 迭代法 用栈模拟递归
class Solution:
    def flatten(self, root: Optional[TreeNode]) -> None:
        if not root:
            return
        stack = [root]  # 初始化栈，存放根节点
        prev = None  # 记录前一个访问的节点
        while stack:
            node = stack.pop()  # 取出当前节点
            # 如果有前一个节点，就把它的 right 指向当前节点
            if prev:
                prev.right = node
                prev.left = None  # 确保 left 为空
            # 先压入 **右子树**，再压入 **左子树**，这样出栈顺序才符合前序遍历
            if node.right:
                stack.append(node.right)
            if node.left:
                stack.append(node.left)
            # 更新 prev 位置
            prev = node
```


```python
# 头插法
class Solution:
    head = None
    def flatten(self, root: Optional[TreeNode]) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        if root is None :return None
        self.flatten(root.right)
        self.flatten(root.left)
        root.left=None
        root.right = self.head
        self.head = root
```
