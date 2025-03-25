## 两数相加
- dummy哨兵节点的赋值
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        cur =  dummy = ListNode() # dummy_head

        carry = 0
        while (l1 or l2):
            val1 = l1.val if l1 else 0
            val2 = l2.val if l2 else 0
            sum = carry + val1 + val2
            res = sum % 10
            carry = sum // 10
            cur.next = ListNode(res)
            cur = cur.next
            
            # cur = cur.next 是重新赋值 cur 变量，而不是修改 cur 原来指向的对象。
            # Python 中的变量是引用，cur = cur.next 只是让 cur 指向一个新的对象，不会影响 dummy 的指向。
            # dummy 始终指向虚拟头节点，而 cur 只是一个移动的指针，用于构建链表。
            if l1:
                l1 = l1.next 
            if l2:
                l2 = l2.next 

        if carry == 1: 
            cur.next = ListNode(carry)
        return dummy.next
            
```
