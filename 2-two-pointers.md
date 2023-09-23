# Two Pointers
## Introduction
Two pointers is a technique that uses two pointers which iterate through the array in tandem to solve problems. The two pointers can move in the same direction or in different directions depending on the problem. The technique is often used as a sub-procedure in other algorithms such as [binary search](./README.md) and [merge sort](./README.md).

## Leetcode Problems using Two Pointers
- [1. Two Sum](https://leetcode.com/problems/two-sum/)
- [15. 3Sum](https://leetcode.com/problems/3sum/)
- [16. 3Sum Closest](https://leetcode.com/problems/3sum-closest/)
- [18. 4Sum](https://leetcode.com/problems/4sum/)
- [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
- [27. Remove Element](https://leetcode.com/problems/remove-element/)
- [28. Implement strStr()](https://leetcode.com/problems/implement-strstr/)
- [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)
- [75. Sort Colors](https://leetcode.com/problems/sort-colors/)
- [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)
- [125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
- [167. Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

## 1. Two Sum
Given an array of integers, return indices of the two numbers such that they add up to a specific target.
```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
    d = {}
    for i, num in enumerate(nums):
        if target - num in d:
            return [d[target - num], i]
        d[num] = i
```
Time complexity: O(n)
Space complexity: O(n)

## 15. 3Sum
Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.
```python
def threeSum(self, nums: List[int]) -> List[List[int]]:
    nums.sort()
    res = []
    for i in range(len(nums)-2):
        if i > 0 and nums[i] == nums[i-1]:
            continue
        l, r = i+1, len(nums)-1
        while l < r:
            s = nums[i] + nums[l] + nums[r]
            if s < 0:
                l += 1
            elif s > 0:
                r -= 1
            else:
                res.append((nums[i], nums[l], nums[r]))
                while l < r and nums[l] == nums[l+1]:
                    l += 1
                while l < r and nums[r] == nums[r-1]:
                    r -= 1
                l += 1; r -= 1
    return res
```
Time complexity: O(n^2)
Space complexity: O(n)

<p align="right"><a href="#">Back to top</a></p>

## 16. 3Sum Closest
Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.
```python
def threeSumClosest(self, nums: List[int], target: int) -> int:
    nums.sort()
    res = sum(nums[:3])
    for i in range(len(nums)-2):
        l, r = i+1, len(nums)-1
        while l < r:
            s = nums[i] + nums[l] + nums[r]
            if s == target:
                return s
            if abs(s - target) < abs(res - target):
                res = s
            if s < target:
                l += 1
            elif s > target:
                r -= 1
    return res
```
Time complexity: O(n^2)
Space complexity: O(n)

<p align="right"><a href="#">Back to top</a></p>

## 18. 4Sum
Given an array nums of n integers and an integer target, are there elements a, b, c, and d in nums such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.
```python
def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
    nums.sort()
    res = []
    for i in range(len(nums)-3):
        if i > 0 and nums[i] == nums[i-1]:
            continue
        for j in range(i+1, len(nums)-2):
            if j > i+1 and nums[j] == nums[j-1]:
                continue
            l, r = j+1, len(nums)-1
            while l < r:
                s = nums[i] + nums[j] + nums[l] + nums[r]
                if s < target:
                    l += 1
                elif s > target:
                    r -= 1
                else:
                    res.append((nums[i], nums[j], nums[l], nums[r]))
                    while l < r and nums[l] == nums[l+1]:
                        l += 1
                    while l < r and nums[r] == nums[r-1]:
                        r -= 1
                    l += 1; r -= 1
    return res
```
Time complexity: O(n^3)
Space complexity: O(n)

<p align="right"><a href="#">Back to top</a></p>

## 26. Remove Duplicates from Sorted Array
Given a sorted array nums, remove the duplicates in-place such that each element appear only once and return the new length.
```python
def removeDuplicates(self, nums: List[int]) -> int:
    if not nums:
        return 0
    i = 0
    for j in range(1, len(nums)):
        if nums[j] != nums[i]:
            i += 1
            nums[i] = nums[j]
    return i + 1
```
Time complexity: O(n)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>

## 27. Remove Element
Given an array nums and a value val, remove all instances of that value in-place and return the new length.
```python
def removeElement(self, nums: List[int], val: int) -> int:
    i = 0
    for j in range(len(nums)):
        if nums[j] != val:
            nums[i] = nums[j]
            i += 1
    return i
```
Time complexity: O(n)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>

## 28. Implement strStr()
Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.
```python
def strStr(self, haystack: str, needle: str) -> int:
    if not needle:
        return 0
    for i in range(len(haystack)-len(needle)+1):
        if haystack[i:i+len(needle)] == needle:
            return i
    return -1
```
Time complexity: O(n)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>

## 42. Trapping Rain Water
Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.
```python
def trap(self, height: List[int]) -> int:
    if not height:
        return 0
    l, r = 0, len(height)-1
    lmax, rmax = height[l], height[r]
    res = 0
    while l < r:
        if lmax < rmax:
            l += 1
            if height[l] < lmax:
                res += lmax - height[l]
            else:
                lmax = height[l]
        else:
            r -= 1
            if height[r] < rmax:
                res += rmax - height[r]
            else:
                rmax = height[r]
    return res
```
Time complexity: O(n)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>

## 75. Sort Colors
Given an array with n objects colored red, white or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white and blue.
```python
def sortColors(self, nums: List[int]) -> None:
    """
    Do not return anything, modify nums in-place instead.
    """
    l, r = 0, len(nums)-1
    i = 0
    while i <= r:
        if nums[i] == 0:
            nums[i], nums[l] = nums[l], nums[i]
            l += 1
            i += 1
        elif nums[i] == 2:
            nums[i], nums[r] = nums[r], nums[i]
            r -= 1
        else:
            i += 1
```
Time complexity: O(n)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>

## 88. Merge Sorted Array
Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.
```python
def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
    """
    Do not return anything, modify nums1 in-place instead.
    """
    i, j = m-1, n-1
    k = m+n-1
    while i >= 0 and j >= 0:
        if nums1[i] > nums2[j]:
            nums1[k] = nums1[i]
            i -= 1
        else:
            nums1[k] = nums2[j]
            j -= 1
        k -= 1
    while j >= 0:
        nums1[k] = nums2[j]
        j -= 1
        k -= 1
```
Time complexity: O(n)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>
