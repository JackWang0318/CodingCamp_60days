```python

class Solution:
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        '''

            先求链表长度l

            p0记录每一段翻转部分前的上一个节点
            pre, cur, nxt负责翻转的操作 
            翻转完后的部分 pre表示最后一个节点 cur为翻转部分之后的下一个节点
            p0也要更新为下一次翻转部分的上一个节点
        '''
        n = 0
        cur = head
        while cur:
            n += 1
            cur = cur.next
        print(n)

        dummy = ListNode(next=head) 
        p0 = dummy
        while n >= k:
            n -= k
            pre = None
            cur = p0.next
            for _ in range(k):
                nxt = cur.next
                cur.next = pre
                pre = cur
                cur = nxt
            # 在翻转本部分之后 把p0更新成下一次待翻转部分的前一个节点
            tmp = p0.next
            p0.next.next = cur
            p0.next = pre
            p0 = tmp

        return dummy.next
```
