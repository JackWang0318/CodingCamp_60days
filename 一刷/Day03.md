
## 链表
### 《代码随想录》链表：移除链表元素
#### 移除链表 
今天发生了很需要反思的事情，下午眯了一会冲个澡打起精神继续打卡leetcode了。
很久没刷链表相关的题目了，也是从原来的c++重新熟悉python语句开始
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        if head is None:
            return None
        dummy_head = ListNode(val=0,next=head)
        cur = dummy_head
               while cur.next != None: # while循环的运行条件的设定
            if cur.next.val == val:
                cur.next = cur.next.next # 注意此处cur是不动的，只改next指针
            else:
                cur = cur.next # cur向后遍历
        return dummy_head.next
```


### 《代码随想录》链表：翻转链表
#### 反转链表
双指针法 pre指针 和cur指针
自己画一个gif就懂了

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        cur = head
        pre = None
        while cur:
            tmp = cur.next
            cur.next = pre
            pre = cur
            cur = tmp
        return pre

```

