## 二叉搜索树的修剪
![image](https://github.com/user-attachments/assets/bbb93f6e-773d-4751-b2d9-ca1af160d9b1)

```python
class Solution:
    def trimBST(self, root: TreeNode, low: int, high: int) -> TreeNode:
        if root is None:
            return None
        if root.val < low:
            # 寻找符合区间 [low, high] 的节点
            return self.trimBST(root.right, low, high)
        if root.val > high:
            # 寻找符合区间 [low, high] 的节点
            return self.trimBST(root.left, low, high)
        # 运行到这 说明root节点符合区间[low,high], 那么继续对root 的left right孩子找符合条件的子树
        root.left = self.trimBST(root.left, low, high)  # root.left 接入符合条件的左孩子
        root.right = self.trimBST(root.right, low, high)  # root.right 接入符合条件的右孩子
        return root

```

## 有序数组构造平衡二叉搜索树

```python
class Solution:
    def traversal(self, nums: List[int], left: int, right: int) -> TreeNode:
        if left > right:
            return None
        
        mid = left + (right - left) // 2
        # print(f'## cur节点 {nums[mid]}')
        root = TreeNode(nums[mid])
        # print(f'# 遍历区间{nums[left:mid-1+1]}')
        root.left = self.traversal(nums, left, mid - 1)
        # print(f'# 遍历区间{nums[mid+1:right+1]}')

        root.right = self.traversal(nums, mid + 1, right)
        return root
    
    def sortedArrayToBST(self, nums: List[int]) -> TreeNode:
        root = self.traversal(nums, 0, len(nums) - 1)
        return root
```

## 二叉搜索树转化累加树

```python
class Solution:
    # 转换成数组 就很好理解 
    # 右中左 反中序遍历
    def convertBST(self, root: TreeNode) -> TreeNode:
        self.pre = 0  # 记录前一个节点的数值
        self.traversal(root)
        return root
    def traversal(self, cur):
        if cur is None:
            return        
        # 右
        self.traversal(cur.right) 
        # 中
        cur.val += self.pre # cur累加
        self.pre = cur.val  # pre更新
        # 左
        self.traversal(cur.left) 
    
```
