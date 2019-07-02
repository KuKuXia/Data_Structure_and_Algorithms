# 二叉树

[TOC]

## 数的遍历

### 遍历种类 

顺序都是相对的根节点来说的。

1. 前序遍历
2. 中序遍历
3. 后序遍历
4. 层序遍历



### 前序遍历

1. 对于二叉搜索树，可以通过中序遍历得到一个递增的有序序列。

   ```python
   # 递归
   def preorderTraversal(root: TreeNode) -> List[int]:
       def preorderTree(root):
           if root is None:
               return
           result.append(root.val)
           preorderTree(root.left)
           preorderTree(root.right)
   
       result = []
       preorderTree(root)
       return result
   
   # 迭代
   def preorderTraversal(root: TreeNode) -> List[int]:
       result, stack = [], []
       if (not root):
           return result
       stack.append(root)
       while(stack):
           # 出栈
           cur = stack.pop()
           if cur:
               result.append(cur.val)
               if(cur.right):
                   stack.append(cur.right)
               if(cur.left):
                   stack.append(cur.left)
        return result    
   ```

   

### 中序遍历

1. 当删除树中的节点时，删除过程将按照后序遍历的顺序进行，即删除操作时，先删除它的左节点，然后删除右节点，最后删除节点本身。

   ```python
   # 递归
   def inorderTraversal(root: TreeNode) -> List[int]:
       def inorderTree(root):
           if not root:
               return
           inorderTree(root.left)
           result.append(root.val)
           inorderTree(root.right)
   
       result = []
       inorderTree(root)
       return result
   # 迭代
   def inorderTraversal(root: TreeNode) -> List[int]:
       result, stack = [], []
       if not root:
           return result
   
       while(stack or root):
           if root:
               stack.append(root)
               root = root.left
           else:
               root = stack.pop()
               result.append(root.val)
               root = root.right
       return result        
   ```

   

### 后序遍历

1. 在数学中解析后缀表示法时使用后序遍历。

   ```python
   # 递归
   def postorderTraversal(self, root: TreeNode) -> List[int]:
       def postorderTree(root):
           if not root:
               return
           postorderTree(root.left)
           postorderTree(root.right)
           result.append(root.val)
   
       result = []
       postorderTree(root)
       return result
   
   # 迭代
   def postorderTraversal(self, root: TreeNode) -> List[int]:
       result, stack = [], []
       if not root:
           return result
   
       p = root
   
       # 辅助指针用来标记最近访问的结点
       flag = None 
   
       while( p or stack):
           if p:
               # 走到最左边
               stack.append(p)
               p = p.left
           else:
               # 向右
               # 获取栈顶
               p = stack[-1]
               # 若右子树存在且为被访问过
               if p.right and p.right != flag:
                   # 转向右
                   p = p.right
                   stack.append(p)
                   # 在走到最左
                   p = p.left
               else:
                   # 访问
                   p = stack.pop()
                   result.append(p.val)
                   # 记录最近访问的结点
                   flag = p
                   # 结点访问结束后重置指针p
                   p = None
      return result
   ```



### 层序遍历

当我们在树中进行广度优先搜索时，我们访问的节点的顺序是按照层序遍历顺序的。

```python'
# dfs + stack
stack = [(root, 0)]
res = []
while stack:
    node, level = stack.pop()
    if node:
        if len(res) < level + 1:
        	res.insert(0, [])
        res[-(level + 1)].append(node.val)
        stack.append((node.right, level + 1))
        stack.append((node.left, level + 1))
return res[::-1]
```

## 递归求解树问题

### 自顶向下

“自顶向下” 意味着在每个递归层级，我们将首先访问节点来计算一些值，并在递归调用函数时将这些值传递到子节点。 所以 “自顶向下” 的解决方案可以被认为是一种**前序遍历**。 具体来说，递归函数 `top_down(root, params)` 的原理是这样的：

```
return specific value for null node
update the answer if needed                      // anwer <-- params
left_ans = top_down(root.left, left_params)      // left_params <-- root.val, params
right_ans = top_down(root.right, right_params)   // right_params <-- root.val, params
return the answer if needed                      // answer <-- left_ans, right_
```



例如给定一个二叉树，求解它的最大深度：

```python
def maxDepth(self, root: TreeNode) -> int:
    self.answer = 0
    
    def maximum_depth(root, depth):
        if not root:
        	return

        if not root.right and not root.left:
        	self.answer = max([self.answer,depth])

        maximum_depth(root.left, depth+1)
        maximum_depth(root.right, depth+1)

    maximum_depth(root, 1)
    return self.answer
```

### 自底向上

“自底向上” 是另一种递归方法。 在每个递归层次上，我们首先对所有子节点递归地调用函数，然后根据返回值和根节点本身的值得到答案。 这个过程可以看作是**后序遍历**`bottom_up(root)`

```
return specific value for null node
left_ans = bottom_up(root.left)          // call function recursively for left child
right_ans = bottom_up(root.right)        // call function recursively for right child
return answers      
```

例如给定一个二叉树，求解它的最大深度：

```python
def maxDepth(self, root: TreeNode) -> int:

    if not root:   # return 0 for null node
    	return 0

    left_depth = self.maxDepth(root.left)
    right_depth = self.maxDepth(root.right)
    return max([left_depth, right_depth]) + 1  # return depth of the subtree at root
```

## LeetCode 101: Symmetric Tree

```python
# 递归
def isSymmetric(self, root: TreeNode) -> bool:
    def isSym(L, R):
        if not L and not R:
            return True
        if L and R and L.val == R.val
        	return isSym(L.left, R.right) and isSym(L.right, R.left)
       	return False
    return isSym(root, root)

# 迭代
from queue import Queue
def isSymmetric(self, root: TreeNode) -> bool:
    if not root:
        return True

    q = Queue()
    q.put(root.left)
    q.put(root.right)

    while(not q.empty()):
        left = q.get(False)
        right = q.get(False)

        if not left and not right: continue
            if not left or not right: return False
            if left.val != right.val: return False
            q.put(left.left)
            q.put(right.right)
            q.put(left.right)
            q.put(right.left)
    return True
```

### LeetCode 112: Path Sum

```python
def hasPathSum(self, root: TreeNode, sum: int) -> bool:
    if not root:
        return False

    if not root.left and not root.right and root.val == sum:
        return True
    sum -= root.val
    return self.hasPathSum(root.left, sum) or self.hasPathSum(root.right, sum)
```

### LeetCode 106: 中序+后序->Tree

```python
def buildTree(inorder: List[int], postorder: List[int]) -> TreeNode:
    if not inorder or not postorder:
        return False
    
    root = TreeNode(postorder.pop())
    inorderIndex = inorder.index(root.val)
    
    root.right = self.buildTree(indorder[inorderIndex+1], postorder)
    root.left = self.buildTree(inorder[:inorderIndex], postorder)
    
    return root
```





































