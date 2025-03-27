## 反转链表II
![image](https://github.com/user-attachments/assets/24f4ff3d-e0e5-402f-80d3-1817bb096b8f)

```python
class Solution:
    def reverseBetween(self, head: Optional[ListNode], left: int, right: int) -> Optional[ListNode]:
        '''
            p0记录翻转部分前的最后一个节点
            pre, cur, tmp负责翻转的操作 
            翻转完后的部分 pre表示最后一个节点 cur为翻转部之后的下一个节点
        '''
        dummy = ListNode(next=head) # 因为最后要返回dummy.next 
        p0 = dummy
        for _ in range(left-1):
            p0 = p0.next

        pre = None
        cur = p0.next
        for _ in range(right-left+1):
            nxt = cur.next
            cur.next = pre
            pre = cur
            cur = nxt
        
        p0.next.next = cur
        p0.next = pre

        return dummy.next
```
