# 面试题07. 重建二叉树

中等 同主站第105题

```
输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

 

例如，给出

前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
返回如下的二叉树：

    3
   / \
  9  20
    /  \
   15   7
```

前序遍历特点： 节点按照 [ 根节点 | 左子树 | 右子树 ] 排序，以题目示例为例：[ 3 | 9 | 20 15 7 ]
中序遍历特点： 节点按照 [ 左子树 | 根节点 | 右子树 ] 排序，以题目示例为例：[ 9 | 3 | 15 20 7 ]



根据以上特点，可以按顺序完成以下工作：

​	前序遍历的首个元素即为根节点 root 的值；
​	在中序遍历中搜索根节点 root 的索引 ，可将中序遍历划分为 [ 左子树 | 根节点 | 右子树 ] 。

​	**根据中序遍历中的左（右）子树的节点数量**，可将前序遍历划分为 [ 根节点 | 左子树 | 右子树 ] 。
​	**自此可确定 三个节点的关系 ：1.树的根节点、2.左子树根节点、3.右子树根节点（即前序遍历中左（右）子	树的首个元素）。**

**因此可用递归来处理，每轮都可以确定3个节点的关系，即当前的根节点和其左右子树的根节点**



## 递归过程详解：

		### 递推参数： 

前序遍历中根节点的索引 **index** 、中序遍历左边界 **left**、中序遍历右边界 **right**。

		### 终止条件：

 当 **left > right** ，子树中序遍历为空，说明已经越过叶子节点，此时返回 None 。

		### 递推工作：

  		1. 建立根节点root： 值为前序遍历中索引为index的节点值。
  		2. 搜索根节点root在中序遍历的索引i： 为了提升搜索效率，本题解使用哈希表 map 预存储中序遍历的值与索引的映射关系，每次搜索的时间复杂度为 O(1)。
  		3. 构建根节点root的左子树和右子树： 通过调用 recur() 方法开启下一层递归。
           **左子树**： 根节点索引为 **index + 1** ，中序遍历的左右边界分别为**left** 和 **index - 1**。
       	**右子树**： 根节点索引为 **index + inorder_index - left + 1**（即：根节点索引 + 左子树长度 + 1），中序遍历的左右边界分别为 **index + 1** 和 **right**。
       	**返回值**： 返回 **root**，含义是当前递归层级建立的根节点 root 为上一递归层级的根节点的左或右子点。

## 难点

难点主要在于确认开启下层递归的3个参数的具体值。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
     def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        map = {num:i for i, num in enumerate(inorder)}	# 建立中序遍历的值与索引的映射

        def recur(index, left, right):	# 递归参数：前序遍历中根节点的索引，中序遍历的左右边界索引
            if left > right:
                return None
            root = TreeNode(preorder[index])	# 构建根节点
            inorder_index = map[preorder[index]]	# 取得中序遍历中根节点的索引
            root.left = recur(index + 1, left, inorder_index - 1)	# 确定左子树的根节点，即开启下层递归
            root.right = recur(index + inorder_index - left + 1, inorder_index + 1, right)	# 确定右子树的根节点，即开启下层递归
            return root
        
        return recur(0, 0, len(preorder) - 1)
            
            

```

