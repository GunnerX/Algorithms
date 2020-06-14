# 面试题06. 从尾到头打印链表

简单

```
输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

 

示例 1：

输入：head = [1,3,2]
输出：[2,3,1]
```

栈即可

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reversePrint(self, head: ListNode) -> List[int]:
        stack, res = [], []
        p = head
        while p:
            stack.append(p.val)
            p = p.next
        for i in range(len(stack)):
            res.append(stack.pop())
        return res
```

时间复杂度：O(N)

空间复杂度：O(N)