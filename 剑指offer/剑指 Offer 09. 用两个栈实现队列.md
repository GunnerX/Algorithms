# 剑指 Offer 09. 用两个栈实现队列

简单

```
用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

 

示例 1：

输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
示例 2：

输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```

1. 如果栈2非空，则栈1为空。此时栈2的栈尾为队列头，栈2的栈顶为队列尾。直接弹出栈2尾部元素则为删除队列头元素

2. 如果栈1，2都为空，说明队列为空

3. 如果栈1非空，说明栈2为空。此时栈1的栈顶为队列头，栈1的栈尾为队列尾。只有这个时候才需要把栈1所有元素移到栈2中。

   **注意此时不需要再把元素从栈2转移回栈1。**

```python
class CQueue:

    def __init__(self):
        self.stack1 = []
        self.stack2 = []


    def appendTail(self, value: int) -> None:
        self.stack1.append(value)

    def deleteHead(self) -> int:
        # 如果栈2非空，则栈1为空。此时栈2的栈尾为队列头，栈2的栈顶为队列尾。直接弹出栈2尾部元素则为删除队列头元素
        if self.stack2: 
            return self.stack2.pop()
        # 如果栈1，2都为空，说明队列为空
        if not self.stack1:
            return -1
        # 如果栈1非空，说明栈2为空。此时栈1的栈顶为队列头，栈1的栈尾为队列尾。只有这个时候才需要把栈1所有元素移到栈2中。注意此时不需要再转移回来。
        else:
            while self.stack1:
                self.stack2.append(self.stack1.pop())
        return self.stack2.pop()



# Your CQueue object will be instantiated and called as such:
# obj = CQueue()
# obj.appendTail(value)
# param_2 = obj.deleteHead()
```

