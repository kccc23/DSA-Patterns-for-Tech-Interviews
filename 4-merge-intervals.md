# Merge Intervals
## Introduction
Given a collection of intervals, merge all overlapping intervals.

For example,
Given `[1,3],[2,6],[8,10],[15,18]`,
return `[1,6],[8,10],[15,18]`.

## Solution
```python
class Solution(object):
    def merge(self, intervals):
        """
        :type intervals: List[Interval]
        :rtype: List[Interval]
        """
        if len(intervals) <= 1:
            return intervals
        intervals.sort(key=lambda x: x[0])
        res = []
        res.append(intervals[0])
        for i in range(1, len(intervals)):
            if intervals[i][0] <= res[-1][-1]:
                res[-1][-1] = max(res[-1][-1], intervals[i][-1])
            else:
                res.append(intervals[i])
        return res
```

## Explanation
Sort the intervals by their start value. Then, we take the first interval and compare its end with the next intervals starts. As long as they overlap, we update the end to be the max end of the overlapping intervals. Once we find a non overlapping interval, we can add the previous "extended" interval and start over.

## Leetcode Problems
- [56. Merge Intervals (Medium)](https://leetcode.com/problems/merge-intervals/)
- [57. Insert Interval (Hard)](https://leetcode.com/problems/insert-interval/)
- [252. Meeting Rooms (Easy)](https://leetcode.com/problems/meeting-rooms/)
- [253. Meeting Rooms II (Medium)](https://leetcode.com/problems/meeting-rooms-ii/)
- [228. Summary Ranges (Medium)](https://leetcode.com/problems/summary-ranges/)

## 228. Summary Ranges
Given a sorted integer array without duplicates, return the summary of its ranges.

Example 1:
```
Input:  [0,1,2,4,5,7]

Output: ["0->2","4->5","7"]

Explanation: 0,1,2 form a continuous range; 4,5 form a continuous range.
```

Example 2:
```
Input:  [0,2,3,4,6,8,9]

Output: ["0","2->4","6","8->9"]

Explanation: 2,3,4 form a continuous range; 8,9 form a continuous range.
```


```python
class Solution(object):
    def summaryRanges(self, nums):
        """
        :type nums: List[int]
        :rtype: List[str]
        """
        if not nums:
            return []
        res = []
        start = nums[0]
        for i in range(1, len(nums)):
            if nums[i] != nums[i-1] + 1:
                if nums[i-1] == start:
                    res.append(str(start))
                else:
                    res.append(str(start) + "->" + str(nums[i-1]))
                start = nums[i]
        if nums[-1] == start:
            res.append(str(start))
        else:
            res.append(str(start) + "->" + str(nums[-1]))
        return res
```

Time complexity: O(n)
Space complexity: O(1)

<p align="right"><a href="#">Back to top</a></p>