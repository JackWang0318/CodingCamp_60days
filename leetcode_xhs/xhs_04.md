## 链表形式的两数相加II
- 反转链表
- 递归方式相加链表节点
![image](https://github.com/user-attachments/assets/059fe8ef-3b55-440e-b1fc-79321e8d39dd)

```python

# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, linkedList):
        '''
        Return the reversed verision of linked list itself
        '''
        if linkedList is None or linkedList.next is None:
            return linkedList

        cur = linkedList
        pre = None
        while cur:##
            temp = cur.next
            cur.next = pre
            pre = cur
            cur = temp
        return pre ##

    def addTwoLinkedlist(self, l1, l2, carry=0):
        '''
        Add two list using recursive method, 
        eg:
        l1: [3,4,2,7]
        l2: [4,6,5]
        return [7,0,8,7] Adding from LSB to MSB
        '''
        if l1 is None and l2 is None:  # 递归边界：l1 和 l2 都是空节点
            return ListNode(carry) if carry else None  # 如果进位了，就额外创建一个节点
        if l1 is None:  # 如果 l1 是空的，那么此时 l2 一定不是空节点
            l1, l2 = l2, l1  # 交换 l1 与 l2，保证 l1 非空，从而简化代码
        carry += l1.val + (l2.val if l2 else 0)  # 节点值和进位加在一起
        l1.val = carry % 10  # 每个节点保存一个数位
        carry = carry // 10
        l1.next = self.addTwoLinkedlist(l1.next, l2.next if l2 else None, carry)  # 进位
        return l1


    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        # 先翻转链表
        l1 = self.reverseList(l1)
        l2 = self.reverseList(l2)
        result = self.addTwoLinkedlist(l1, l2)
        result = self.reverseList(result)
        return result





```
