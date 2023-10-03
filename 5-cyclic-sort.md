# Cyclic Sort
## Introduction
Cyclic sort is a pattern that can be used to solve problems involving arrays containing numbers in a given range. For example, take the following problem:

> You are given an unsorted array containing numbers taken from the range 1 to ‘n’. The array can have duplicates, which means some numbers will be missing. Find all the missing numbers.

To efficiently solve this problem, we can use the fact that the input array contains numbers in the range of 1 to ‘n’. For example, to efficiently sort the array, we can try placing each number in its correct place, i.e., placing ‘1’ at index ‘0’, placing ‘2’ at index ‘1’, and so on. Once we are done with the sorting, we can iterate the array to find all indices that are missing the correct numbers. These will be our required numbers.

## LeetCode Problems
<!-- leetcode problems with their links on leetcode.com -->
- [268. Missing Number](https://leetcode.com/problems/missing-number/)
- [448. Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)
- [287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)
- [442. Find All Duplicates in an Array](https://leetcode.com/problems/find-all-duplicates-in-an-array/)
- [41. First Missing Positive](https://leetcode.com/problems/first-missing-positive/)

### Problem 1: Find the Missing Number (easy)
We are given an array containing ‘n’ distinct numbers taken from the range 0 to ‘n’. Since the array has only ‘n’ numbers out of the total ‘n+1’ numbers, find the missing number.

Example 1:
```
Input: [4, 0, 3, 1]
Output: 2
```
Example 2:
```
Input: [8, 3, 5, 2, 4, 6, 0, 1]
Output: 7
```
Solution:
```python
def find_missing_number(nums):
    i, n = 0, len(nums)
    while i < n:
        j = nums[i]
        if j < n and nums[i] != nums[j]:
            nums[i], nums[j] = nums[j], nums[i]
        else:
            i += 1
    for i in range(n):
        if nums[i] != i:
            return i
    return n
```
Time complexity: O(n)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>

### Problem 2: Find all Missing Numbers (easy)
We are given an unsorted array containing numbers taken from the range 1 to ‘n’. The array can have duplicates, which means some numbers will be missing. Find all those missing numbers.

Example 1:
```
Input: [2, 3, 1, 8, 2, 3, 5, 1]
Output: 4, 6, 7
```
Example 2:
```
Input: [2, 4, 1, 2]
Output: 3
```
Example 3:
```
Input: [2, 3, 2, 1]
Output: 4
```
Solution:
```python
def find_missing_numbers(nums):
    i, n = 0, len(nums)
    while i < n:
        j = nums[i] - 1
        if nums[i] != nums[j]:
            nums[i], nums[j] = nums[j], nums[i]
        else:
            i += 1
    res = []
    for i in range(n):
        if nums[i] != i + 1:
            res.append(i + 1)
    return res
```
Time complexity: O(n)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>

### Problem 3: Find the Duplicate Number (easy)
We are given an unsorted array containing ‘n+1’ numbers taken from the range 1 to ‘n’. The array has only one duplicate but it can be repeated multiple times. Find that duplicate number without using any extra space. You are, however, allowed to modify the input array.

Example 1:
```
Input: [1, 4, 4, 3, 2]
Output: 4
```
Example 2:
```
Input: [2, 1, 3, 3, 5, 4]
Output: 3
```
Example 3:
```
Input: [2, 4, 1, 4, 4]
Output: 4
```
Solution:
```python
def find_duplicate(nums):
    i, n = 0, len(nums)
    while i < n:
        j = nums[i] - 1
        if nums[i] != nums[j]:
            nums[i], nums[j] = nums[j], nums[i]
        else:
            i += 1
    for i in range(n):
        if nums[i] != i + 1:
            return nums[i]
    return -1
```
Time complexity: O(n)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>

### Problem 4: Find all Duplicate Numbers (easy)
We are given an unsorted array containing ‘n’ numbers taken from the range 1 to ‘n’. The array has some duplicates, find all the duplicate numbers without using any extra space.

Example 1:
```
Input: [3, 4, 4, 5, 5]
Output: [4, 5]
```
Example 2:
```
Input: [5, 4, 7, 2, 3, 5, 3]
Output: [3, 5]
```
Solution:
```python
def find_all_duplicates(nums):
    i, n = 0, len(nums)
    while i < n:
        j = nums[i] - 1
        if nums[i] != nums[j]:
            nums[i], nums[j] = nums[j], nums[i]
        else:
            i += 1
    res = []
    for i in range(n):
        if nums[i] != i + 1:
            res.append(nums[i])
    return res
```
Time complexity: O(n)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>