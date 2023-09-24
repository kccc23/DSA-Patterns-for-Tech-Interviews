# Tree Breadth First Search
## Introduction
Breadth First Search (BFS) is an algorithm for traversing or searching tree or graph data structures. It starts at the tree root (or some arbitrary node of a graph, sometimes referred to as a 'search key'), and explores all of the neighbor nodes at the present depth prior to moving on to the nodes at the next depth level.

BFS is a traversing algorithm where you should start traversing from a selected node (source or starting node) and traverse the graph layerwise thus exploring the neighbour nodes (nodes which are directly connected to source node). You must then move towards the next-level neighbour nodes.

## Leetcode Problems using BFS
- [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
- [107. Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)
- [103. Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)
- [199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)
- [637. Average of Levels in Binary Tree](https://leetcode.com/problems/average-of-levels-in-binary-tree/)
- [515. Find Largest Value in Each Tree Row](https://leetcode.com/problems/find-largest-value-in-each-tree-row/)
- [1161. Maximum Level Sum of a Binary Tree](https://leetcode.com/problems/maximum-level-sum-of-a-binary-tree/)
- [1302. Deepest Leaves Sum](https://leetcode.com/problems/deepest-leaves-sum/)
- [993. Cousins in Binary Tree](https://leetcode.com/problems/cousins-in-binary-tree/)
- [111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)

## Binary Tree Level Order Traversal
Given a binary tree, populate an array to represent its level-by-level traversal. You should populate the values of all nodes of each level from left to right in separate sub-arrays.

```python
def levelOrder(self, root: TreeNode) -> List[List[int]]:
    if not root:
        return []
    res = []
    q = deque([root])
    while q:
        level = []
        for _ in range(len(q)):
            node = q.popleft()
            level.append(node.val)
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)
        res.append(level)
    return res
```
Time complexity: O(n)
Space complexity: O(n)

## Reverse Level Order Traversal
Given a binary tree, populate an array to represent its level-by-level traversal in reverse order, i.e., the lowest level comes first. You should populate the values of all nodes in each level from left to right in separate sub-arrays.

```python
def reverseLevelOrder(self, root: TreeNode) -> List[List[int]]:
    if not root:
        return []
    res = deque()
    q = deque([root])
    while q:
        level = []
        for _ in range(len(q)):
            node = q.popleft()
            level.append(node.val)
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)
        res.appendleft(level)
    return res
```
Time complexity: O(n)
Space complexity: O(n)

## Zigzag Traversal
Given a binary tree, populate an array to represent its zigzag level order traversal. You should populate the values of all nodes of the first level from left to right, then right to left for the next level and keep alternating in the same manner for the following levels.

```python
def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
    if not root:
        return []
    res = []
    q = deque([root])
    left_to_right = True
    while q:
        level = deque()
        for _ in range(len(q)):
            node = q.popleft()
            if left_to_right:
                level.append(node.val)
            else:
                level.appendleft(node.val)
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)
        res.append(list(level))
        left_to_right = not left_to_right
    return res
```
Time complexity: O(n)
Space complexity: O(n)

## Level Averages in a Binary Tree
Given a binary tree, populate an array to represent the averages of all of its levels.

```python
def averageOfLevels(self, root: TreeNode) -> List[float]:
    if not root:
        return []
    res = []
    q = deque([root])
    while q:
        level = []
        for _ in range(len(q)):
            node = q.popleft()
            level.append(node.val)
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)
        res.append(sum(level) / len(level))
    return res
```
Time complexity: O(n)
Space complexity: O(n)

## Minimum Depth of a Binary Tree
Find the minimum depth of a binary tree. The minimum depth is the number of nodes along the shortest path from the root node to the nearest leaf node.

```python
def minDepth(self, root: TreeNode) -> int:
    if not root:
        return 0
    q = deque([root])
    depth = 0
    while q:
        depth += 1
        for _ in range(len(q)):
            node = q.popleft()
            if not node.left and not node.right:
                return depth
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)
    return depth
```
Time complexity: O(n)
Space complexity: O(n)

## Level Order Successor
Given a binary tree and a node, find the level order successor of the given node in the tree. The level order successor is the node that appears right after the given node in the level order traversal.

```python
def findSuccessor(self, root: TreeNode, key: int) -> TreeNode:
    if not root:
        return None
    q = deque([root])
    while q:
        for _ in range(len(q)):
            node = q.popleft()
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)
            if node.val == key:
                return q[0] if q else None
    return None
```
Time complexity: O(n)
Space complexity: O(n)
