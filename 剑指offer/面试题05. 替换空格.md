# 面试题05. 替换空格

简单

```
请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

 

示例 1：

输入：s = "We are happy."
输出："We%20are%20happy."
```

使用append方法

```python
class Solution:
    def replaceSpace(self, s: str) -> str:
        bytes = []
        for char in s:
            if char == ' ':
                bytes.append('%20')
            else:
                bytes.append(char)
        return ''.join(bytes)
```

如果不能用append方法，则创建一个大小为 3 * len(s) 的字符数组保存结果，用指针i来遍历并更新bytes中的字符

```python
class Solution:
    def replaceSpace(self, s: str) -> str:
        new_len = 3 * len(s)
        bytes = [''] * new_len

        i = 0   # i指针用来在新的bytes中移动
        for char in s:
            if char == ' ':
                bytes[i] = '%'
                bytes[i+1] = '2'
                bytes[i+2] = '0'
                i += 3
            else:
                bytes[i] = char
                i += 1
        # 去掉bytes末尾多余的字符
        for j in range(i, new_len - 1):
            bytes.pop()
            
        return ''.join(bytes)
```

