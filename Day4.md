
### 《代码随想录》链表：两两交换链表中的节点
#### 两支颜色的笔画图厘清tmp1和tmp2
两周前刚做过本题，当时还有点晕乎 以为是两对(1和2，3和4)同时交换节点，题目没看清楚。这次明显有知错就改的感觉，很快纸上画了交换顺序就搞定了，就是调试的时候cur.next.next要注意 不是cur.next*3*，因为可能链表就一个节点*

```python
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy_head = ListNode(next=head)
        cur = dummy_head
        while cur.next and cur.next.next:
            tmp1 = cur.next
            tmp2 = cur.next.next.next
            cur.next = cur.next.next
            cur.next.next = tmp1
            cur.next.next.next = tmp2
            cur = cur.next.next        
        return dummy_head.next
```

### 《代码随想录》链表：删除链表的倒数第N个节点
#### 双指针法 巧妙转化
倒数n可以转化为正向遍历时快慢指针之差的思想 
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy_head = ListNode(next=head)
        slow = dummy_head
        fast = dummy_head
        for i in range(n):
            fast = fast.next
        while fast.next != None:
            slow = slow.next
            fast = fast.next
        slow.next = slow.next.next
        return dummy_head.next
```

### 《代码随想录》链表：环形链表II
#### 第一反应是用一个集合统计访问节点，后面才想起来有数学推导后的快慢指针

```python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = head
        fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                x = head
                y = slow # or fast
                while x != y:
                    x = x.next
                    y = y.next
                return x
        return None
```
