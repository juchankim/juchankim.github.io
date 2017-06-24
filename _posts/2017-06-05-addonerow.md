
Given the root of a binary tree, then value v and depth d, you need to add a row of nodes with value v at the given depth d. The root node is at depth 1.

https://leetcode.com/problems/add-one-row-to-tree

```python

# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def addOneRow(self, root, v, d):
        """
        :type root: TreeNode
        :type v: int
        :type d: int
        :rtype: TreeNode
        """
        def helper(root, v, d, leftRight): #true = left, false = right
            if d == 1:
                a = TreeNode(v)
                if leftRight:
                    a.left = root
                else:
                    a.right = root
                return a
            else:
                if root:
                    root.left = helper(root.left, v, d-1, True)
                    root.right = helper(root.right, v, d-1, False)
                return root
                
        return helper(root, v, d, True)
```