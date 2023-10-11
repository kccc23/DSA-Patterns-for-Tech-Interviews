# In-Place Reversal of a LinkedList
## Problem Statement
Given the head of a Singly LinkedList, reverse the LinkedList. Write a function to return the new head of the reversed LinkedList.
## Explanation
The problem is quite straightforward. We can follow the following steps to reverse a LinkedList:
1. Take the head of the LinkedList and one by one iterate through all nodes of the LinkedList, putting them at the beginning of the reversed LinkedList.
2. We will use a variable called `current` to store the current node that we will be processing and a variable called `previous` to store the previous node that we have processed. At each step, we will insert the current node to the beginning of the reversed LinkedList and update `current` to point to the next node that needs to be processed. The insertion at the beginning of the reversed LinkedList will take O(1) time.
3. After the current node is inserted at the beginning of the reversed LinkedList, we’ll update the `previous` to always point to the current node before moving `current` to the next node.
4. In the end, `current` will point to `null`, indicating that we have reached the end of the LinkedList.
5. We will point the head of the LinkedList to the `previous` node, which will be the new head of the reversed LinkedList.
6. The time complexity of our algorithm will be O(N) where ‘N’ is the total number of nodes in the LinkedList.
7. The algorithm runs in constant space O(1).

## Code
```python
class Node:
    def __init__(self, value, next=None):
        self.value = value
        self.next = next

    def print_list(self):
        temp = self
        while temp is not None:
            print(str(temp.value) + " ", end='')
            temp = temp.next
        print()
```
```python
def reverse(head):
    previous, current, next = None, head, None
    while current is not None:
        next = current.next  # temporarily store the next node
        current.next = previous  # reverse the current node
        previous = current  # before we move to the next node, point previous to the current node
        current = next  # move on the next node
    return previous
```
```python
def main():
    head = Node(2)
    head.next = Node(4)
    head.next.next = Node(6)
    head.next.next.next = Node(8)
    head.next.next.next.next = Node(10)

    print("Nodes of original LinkedList are: ", end='')
    head.print_list()
    result = reverse(head)
    print("Nodes of reversed LinkedList are: ", end='')
    result.print_list()
```

## Time & Space Complexity
The time complexity of our algorithm will be O(N) where ‘N’ is the total number of nodes in the LinkedList.
The algorithm runs in constant space O(1).
## Similar Problems
### Problem 1: Reverse a Sub-list (medium)
Given the head of a LinkedList and two positions ‘p’ and ‘q’, reverse the LinkedList from position ‘p’ to ‘q’.

```python
def reverse_sub_list(head, p, q):
    if p == q:
        return head

    # after skipping 'p-1' nodes, current will point to 'p'th node
    current, previous = head, None
    i = 0
    while current is not None and i < p - 1:
        previous = current
        current = current.next
        i += 1

    # we are interested in three parts of the LinkedList, the part before index 'p',
    # the part between 'p' and 'q', and the part after index 'q'
    last_node_of_first_part = previous
    # after reversing the LinkedList 'current' will become the last node of the sub-list
    last_node_of_sub_list = current
    next = None  # will be used to temporarily store the next node

    i = 0
    # reverse nodes between 'p' and 'q'
    while current is not None and i < q - p + 1:
        next = current.next
        current.next = previous
        previous = current
        current = next
        i += 1

    # connect with the first part
    if last_node_of_first_part is not None:
        # 'previous' is now the first node of the sub-list
        last_node_of_first_part.next = previous
    # this means p == 1 i.e., we are changing the first node (head) of the LinkedList
    else:
        head = previous

    # connect with the last part
    last_node_of_sub_list.next = current
    return head
```

### Problem 2: Reverse every K-element Sub-list (medium)
Given the head of a LinkedList and a number ‘k’, reverse every ‘k’ sized sub-list starting from the head.

```python
def reverse_every_k_elements(head, k):
    if k <= 1 or head is None:
        return head

    current, previous = head, None
    while True:
        last_node_of_previous_part = previous
        # after reversing the LinkedList 'current' will become the last node of the sub-list
        last_node_of_sub_list = current
        next = None  # will be used to temporarily store the next node

        i = 0
        while current is not None and i < k:  # reverse 'k' nodes
            next = current.next
            current.next = previous
            previous = current
            current = next
            i += 1

        # connect with the previous part
        if last_node_of_previous_part is not None:
            last_node_of_previous_part.next = previous
        else:
            head = previous

        # connect with the next part
        last_node_of_sub_list.next = current

        if current is None:
            break
        previous = last_node_of_sub_list
    return head
```

### Problem 3: Reverse alternating K-element Sub-list (medium)
Given the head of a LinkedList and a number ‘k’, reverse every alternating ‘k’ sized sub-list starting from the head.

```python
def reverse_alternate_k_elements(head, k):
    if k <= 1 or head is None:
        return head

    current, previous = head, None
    while True:
        last_node_of_previous_part = previous
        # after reversing the LinkedList 'current' will become the last node of the sub-list
        last_node_of_sub_list = current
        next = None  # will be used to temporarily store the next node

        i = 0
        while current is not None and i < k:  # reverse 'k' nodes
            next = current.next
            current.next = previous
            previous = current
            current = next
            i += 1

        # connect with the previous part
        if last_node_of_previous_part is not None:
            last_node_of_previous_part.next = previous
        else:
            head = previous

        # connect with the next part
        last_node_of_sub_list.next = current

        # skip 'k' nodes
        i = 0
        while current is not None and i < k:
            previous = current
            current = current.next
            i += 1

        if current is None:
            break
    return head
```

### Problem 4: Rotate a LinkedList (medium)
Given the head of a Singly LinkedList and a number ‘k’, rotate the LinkedList to the right by ‘k’ nodes.

```python
def rotate(head, rotations):
    if head is None or head.next is None or rotations <= 0:
        return head

    # find the length and the last node of the list
    last_node = head
    list_length = 1
    while last_node.next is not None:
        last_node = last_node.next
        list_length += 1

    last_node.next = head  # connect the last node with the head to make it a circular list
    rotations %= list_length  # no need to do rotations more than the length of the list
    skip_length = list_length - rotations
    last_node_of_rotated_list = head
    for i in range(skip_length - 1):
        last_node_of_rotated_list = last_node_of_rotated_list.next

    # 'last_node_of_rotated_list.next' is pointing to the sub-list of 'k' ending nodes
    head = last_node_of_rotated_list.next
    last_node_of_rotated_list.next = None
    return head
```
