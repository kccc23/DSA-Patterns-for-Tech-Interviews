# Fast and Slow Pointers
## Introduction
Fast and Slow pointers is a pattern that uses two pointers which move through the array (or sequence/LinkedList) at different speeds, also known as the Hare & Tortoise Algorithm. This approach is quite useful when dealing with cyclic LinkedLists or arrays.

By moving at different speeds (say, in a cyclic LinkedList), the algorithm proves that the two pointers are bound to meet. The fast pointer should catch the slow pointer once both the pointers are in a cyclic loop.

## Leetcode Problems using Fast and Slow Pointers
- [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
- [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)
- [202. Happy Number](https://leetcode.com/problems/happy-number/)
- [876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)
- [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)
- [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
- [457. Circular Array Loop](https://leetcode.com/problems/circular-array-loop/)
- [109. Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)
- [1417. Reformat The String](https://leetcode.com/problems/reformat-the-string/)
- [1423. Maximum Points You Can Obtain from Cards](https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/)

## 141. Linked List Cycle
Given head, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, `pos` is used to denote the index of the node that tail's next pointer is connected to. Note that `pos` is not passed as a parameter.

Return `true` if there is a cycle in the linked list. Otherwise, return `false`.

```python
def hasCycle(self, head: ListNode) -> bool:
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```
Time complexity: O(n)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>

## 142. Linked List Cycle II
Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

Notice that you should not modify the linked list.

```python
def detectCycle(self, head: ListNode) -> ListNode:
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            break
    else:
        return None
    while head != slow:
        head = head.next
        slow = slow.next
    return head
```
Time complexity: O(n)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>

## 202. Happy Number
Write an algorithm to determine if a number `n` is happy.

A **happy number** is a number defined by the following process:

- Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals 1 (where it will stay), or it **loops endlessly in a cycle** which does not include 1.
- Those numbers for which this process **ends in 1** are happy.
- Return `true` if `n` is a happy number, and `false` if not.

```python
def isHappy(self, n: int) -> bool:
    def get_next(n):
        total_sum = 0
        while n > 0:
            n, digit = divmod(n, 10)
            total_sum += digit ** 2
        return total_sum
    slow = fast = n
    while True:
        slow = get_next(slow)
        fast = get_next(get_next(fast))
        if slow == fast:
            break
    return slow == 1
```
Time complexity: O(logn)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>

## 876. Middle of the Linked List
Given the head of a singly linked list, return the middle node of the linked list.

If there are two middle nodes, return the second middle node.

```python
def middleNode(self, head: ListNode) -> ListNode:
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow
```
Time complexity: O(n)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>

## 234. Palindrome Linked List
Given the head of a singly linked list, return true if it is a palindrome.

```python
def isPalindrome(self, head: ListNode) -> bool:
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    prev = None
    while slow:
        slow.next, prev, slow = prev, slow, slow.next
    while prev:
        if prev.val != head.val:
            return False
        prev = prev.next
        head = head.next
    return True
```
Time complexity: O(n)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>

## 19. Remove Nth Node From End of List
Given the head of a linked list, remove the nth node from the end of the list and return its head.

```python
def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
    dummy = ListNode(0, head)
    slow = fast = dummy
    for _ in range(n):
        fast = fast.next
    while fast and fast.next:
        slow = slow.next
        fast = fast.next
    slow.next = slow.next.next
    return dummy.next
```
Time complexity: O(n)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>

## 457. Circular Array Loop
You are given an array of positive and negative integers. If a number n at an index is positive, then move forward n steps. Conversely, if it's negative (-n), move backward n steps. Assume the first element of the array is forward next to the last element, and the last element is backward next to the first element. Determine if there is a loop in this array. A loop starts and ends at a particular index with more than 1 element along the loop. The loop must be "forward" or "backward'.

```python
def circularArrayLoop(self, nums: List[int]) -> bool:
    def next_index(i):
        return (i + nums[i]) % len(nums)
    for i in range(len(nums)):
        if nums[i] == 0:
            continue
        slow = fast = i
        while True:
            slow = next_index(slow)
            fast = next_index(next_index(fast))
            if nums[slow] * nums[fast] < 0 or nums[fast] * nums[next_index(fast)] < 0:
                break
            if slow == fast:
                if slow == next_index(slow):
                    break
                return True
        slow = i
        while nums[slow] * nums[next_index(slow)] > 0:
            nums[slow], slow = 0, next_index(slow)
    return False
```
Time complexity: O(n)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>

## 109. Convert Sorted List to Binary Search Tree
Given the head of a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

```python
def sortedListToBST(self, head: ListNode) -> TreeNode:
    def find_mid(head):
        prev = None
        slow = fast = head
        while fast and fast.next:
            prev = slow
            slow = slow.next
            fast = fast.next.next
        if prev:
            prev.next = None
        return slow
    if not head:
        return None
    mid = find_mid(head)
    root = TreeNode(mid.val)
    if head == mid:
        return root
    root.left = self.sortedListToBST(head)
    root.right = self.sortedListToBST(mid.next)
    return root
```
Time complexity: O(nlogn)
Space complexity: O(logn)

<p align="right"><a href="#">Back to top</a></p>

## 1417. Reformat The String
Given alphanumeric string `s`. (Alphanumeric string is a string consisting of lowercase English letters and digits).

You have to find a permutation of the string where no letter is followed by another letter and no digit is followed by another digit. That is, no two adjacent characters have the same type.

Return the reformatted string or return an empty string if it is impossible to reformat the string.

```python
def reformat(self, s: str) -> str:
    digits = []
    letters = []
    for c in s:
        if c.isdigit():
            digits.append(c)
        else:
            letters.append(c)
    if abs(len(digits) - len(letters)) > 1:
        return ""
    res = []
    if len(digits) > len(letters):
        res.append(digits.pop())
    while digits and letters:
        res.append(digits.pop())
        res.append(letters.pop())
    if digits:
        res.append(digits.pop())
    if letters:
        res.insert(0, letters.pop())
    return "".join(res)
```
Time complexity: O(n)
Space complexity: O(n)

<p align="right"><a href="#">Back to top</a></p>

## 1423. Maximum Points You Can Obtain from Cards
There are several cards arranged in a row, and each card has an associated number of points. The points are given in the integer array `cardPoints`.

In one step, you can take one card from the beginning or from the end of the row. You have to take exactly `k` cards.

Your score is the sum of the points of the cards you have taken.

Given the integer array `cardPoints` and the integer `k`, return the *maximum score* you can obtain.

```python
def maxScore(self, cardPoints: List[int], k: int) -> int:
    total = sum(cardPoints)
    if k == len(cardPoints):
        return total
    n = len(cardPoints)
    window = sum(cardPoints[:n-k])
    res = total - window
    for i in range(n-k, n):
        window += cardPoints[i] - cardPoints[i-n+k]
        res = max(res, total - window)
    return res
```
Time complexity: O(n)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>
