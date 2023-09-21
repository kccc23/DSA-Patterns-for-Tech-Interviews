# Sliding Window  <a id="top"></a>
## 1. Introduction
The sliding window pattern is used to perform a required operation on a specific window size of a given array or linked list such as finding the longest subarray containing all 1s.
## 2. How do we identify that we need to use a sliding window pattern?
The problem will deal with a set of contiguous elements such as a subarray, substring, or a sliding window. When we are given a problem dealing with a set of contiguous elements within a linear data structure, we should immediately think of the sliding window pattern.
## 3. How do we implement the sliding window pattern?
The sliding window pattern is very popular among array and string problems
- We will use a **left** and **right** pointer to create a window that contains a subset of the data in the array/string.
- We will manipulate the **left** and **right** pointers to create different windows and move the window based on the problem constraints.
- We will expand or shrink the window according to the problem constraints and will keep track of the maximum window size that we have seen so far.
- We will not need to create a new array or string to keep track of the subset of data in the window.
- We will just need to use the indices of the original array/string to track the **start** and **end** of the window.
- We will use a **hash table** or a **dictionary** to track the frequency of certain characters or numbers, this will help us to find the required character or number frequencies so that we can shrink or expand our window to find the optimal solution.
- In some problems, we are asked to find the longest substring containing all **repeating** characters or find the longest substring containing **K** distinct characters, these types of problems will require us to expand the window when we have repeating characters or shrink the window when we have distinct characters as a part of the window.
- In some problems, we are asked to find the longest substring containing **no more than K** distinct characters, these types of problems will require us to shrink the window when we have more than **K** distinct characters in the current window.

## Leetcode Problems using Sliding Window
- [643. Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)
- [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)
- [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
- [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)
- [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)


## 1. Maximum Sum Subarray of Size K (easy)
Given an array of positive numbers and a positive number ‘k’, find the maximum sum of any contiguous subarray of size ‘k’.
```python
def max_sub_array_of_size_k(k, arr):
  max_sum, window_sum = 0, 0
  window_start = 0
  for window_end in range(len(arr)):
    window_sum += arr[window_end]  # add the next element
    # slide the window, we don't need to slide if we've not hit the required window size of 'k'
    if window_end >= k - 1:
      max_sum = max(max_sum, window_sum)
      window_sum -= arr[window_start]  # subtract the element going out
      window_start += 1  # slide the window ahead
  return max_sum
```
Time Complexity: O(N)
Space Complexity: O(1)

<p align="right"><a href="#top">Back to top</a></p>

## 2. Smallest Subarray with a given sum (easy)
Given an array of positive numbers and a positive number ‘S’, find the length of the smallest contiguous subarray whose sum is greater than or equal to ‘S’. Return 0, if no such subarray exists.
```python
def smallest_subarray_with_given_sum(s, arr):
  window_sum = 0
  min_length = float("inf")
  window_start = 0
  for window_end in range(0, len(arr)):
    window_sum += arr[window_end]  # add the next element
    # shrink the window as small as possible until the 'window_sum' is smaller than 's'
    while window_sum >= s:
      min_length = min(min_length, window_end - window_start + 1)
      window_sum -= arr[window_start]
      window_start += 1
  if min_length == float("inf"):
    return 0
  return min_length
```
Time Complexity: O(N)
Space Complexity: O(1)

<p align="right"><a href="#top">Back to top</a></p>

## 3. Longest Substring with K Distinct Characters (medium)
Given a string, find the length of the longest substring in it with no more than K distinct characters.
```python
def longest_substring_with_k_distinct(str1, k):
  window_start = 0
  max_length = 0
  char_frequency = {}

  # in the following loop we'll try to extend the range [window_start, window_end]
  for window_end in range(len(str1)):
    right_char = str1[window_end]
    if right_char not in char_frequency:
      char_frequency[right_char] = 0
    char_frequency[right_char] += 1

    # shrink the sliding window, until we are left with 'k' distinct characters in the char_frequency
    while len(char_frequency) > k:
      left_char = str1[window_start]
      char_frequency[left_char] -= 1
      if char_frequency[left_char] == 0:
        del char_frequency[left_char]
      window_start += 1  # shrink the window
    # remember the maximum length so far
    max_length = max(max_length, window_end-window_start + 1)
  return max_length
```
Time Complexity: O(N)
Space Complexity: O(K)

<p align="right"><a href="#top">Back to top</a></p>

## 4. Fruits into Baskets (medium)
Given an array of characters where each character represents a fruit tree, you are given two baskets and your goal is to put maximum number of fruits in each basket. The only restriction is that each basket can have only one type of fruit.
```python
def fruits_into_baskets(fruits):
  window_start = 0
  max_length = 0
  fruit_frequency = {}

  # try to extend the range [window_start, window_end]
  for window_end in range(len(fruits)):
    right_fruit = fruits[window_end]
    if right_fruit not in fruit_frequency:
      fruit_frequency[right_fruit] = 0
    fruit_frequency[right_fruit] += 1

    # shrink the sliding window, until we are left with '2' fruits in the fruit frequency dictionary
    while len(fruit_frequency) > 2:
      left_fruit = fruits[window_start]
      fruit_frequency[left_fruit] -= 1
      if fruit_frequency[left_fruit] == 0:
        del fruit_frequency[left_fruit]
      window_start += 1  # shrink the window
    max_length = max(max_length, window_end-window_start + 1)
  return max_length
```
Time Complexity: O(N)
Space Complexity: O(1)

<p align="right"><a href="#top">Back to top</a></p>

## 5. No-repeat Substring (hard)
Given a string, find the length of the longest substring which has no repeating characters.
```python
def non_repeat_substring(str1):
  window_start = 0
  max_length = 0
  char_index_map = {}

  # try to extend the range [windowStart, windowEnd]
  for window_end in range(len(str1)):
    right_char = str1[window_end]
    # if the map already contains the 'right_char', shrink the window from the beginning so that
    # we have only one occurrence of 'right_char'
    if right_char in char_index_map:
      # this is tricky; in the current window, we will not have any 'right_char' after its previous index
      # and if 'window_start' is already ahead of the last index of 'right_char', we'll keep 'window_start'
      window_start = max(window_start, char_index_map[right_char] + 1)
    # insert the 'right_char' into the map
    char_index_map[right_char] = window_end
    # remember the maximum length so far
    max_length = max(max_length, window_end-window_start + 1)
  return max_length
```
Time Complexity: O(N)
Space Complexity: O(K)

<p align="right"><a href="#top">Back to top</a></p>

## 6. Longest Substring with Same Letters after Replacement (hard)
Given a string with lowercase letters only, if you are allowed to replace no more than ‘k’ letters with any letter, find the length of the longest substring having the same letters after replacement.
```python
def length_of_longest_substring(str1, k):
  window_start, max_length, max_repeat_letter_count = 0, 0, 0
  frequency_map = {}

  # Try to extend the range [window_start, window_end]
  for window_end in range(len(str1)):
    right_char = str1[window_end]
    if right_char not in frequency_map:
      frequency_map[right_char] = 0
    frequency_map[right_char] += 1
    max_repeat_letter_count = max(max_repeat_letter_count, frequency_map[right_char])

    # Current window size is from window_start to window_end, overall we have a letter which is
    # repeating 'max_repeat_letter_count' times, this means we can have a window which has one letter
    # repeating 'max_repeat_letter_count' times and the remaining letters we should replace.
    # if the remaining letters are more than 'k', it is the time to shrink the window as we
    # are not allowed to replace more than 'k' letters
    if (window_end - window_start + 1 - max_repeat_letter_count) > k:
      left_char = str1[window_start]
      frequency_map[left_char] -= 1
      window_start += 1

    max_length = max(max_length, window_end - window_start + 1)
  return max_length
```
Time Complexity: O(N)
Space Complexity: O(1)

<p align="right"><a href="#top">Back to top</a></p>

## 7. Longest Subarray with Ones after Replacement (hard)
Given an array containing 0s and 1s, if you are allowed to replace no more than ‘k’ 0s with 1s, find the length of the longest contiguous subarray having all 1s.
```python
def length_of_longest_substring(arr, k):
  window_start, max_length, max_ones_count = 0, 0, 0

  # Try to extend the range [window_start, window_end]
  for window_end in range(len(arr)):
    if arr[window_end] == 1:
      max_ones_count += 1

    # Current window size is from window_start to window_end, overall we have a maximum of 1s
    # repeating 'max_ones_count' times, this means we can have a window with 'max_ones_count' 1s
    # and the remaining are 0s which should replace with 1s.
    # now, if the remaining 1s are more than 'k', it is the time to shrink the window as we
    # are not allowed to replace more than 'k' 0s
    if (window_end - window_start + 1 - max_ones_count) > k:
      if arr[window_start] == 1:
        max_ones_count -= 1
      window_start += 1

    max_length = max(max_length, window_end - window_start + 1)
  return max_length
```
Time Complexity: O(N)
Space Complexity: O(1)

<p align="right"><a href="#top">Back to top</a></p>

## 8. Permutation in a String (hard)
Given a string and a pattern, find out if the string contains any permutation of the pattern.
```python
def find_permutation(str1, pattern):
  window_start, matched = 0, 0
  char_frequency = {}

  for chr in pattern:
    if chr not in char_frequency:
      char_frequency[chr] = 0
    char_frequency[chr] += 1

  # our goal is to match all the characters from the 'char_frequency' with the current window
  # try to extend the range [window_start, window_end]
  for window_end in range(len(str1)):
    right_char = str1[window_end]
    if right_char in char_frequency:
      # decrement the frequency of matched character
      char_frequency[right_char] -= 1
      if char_frequency[right_char] == 0:
        matched += 1

    if matched == len(char_frequency):
      return True

    # shrink the window by one character
    if window_end >= len(pattern) - 1:
      left_char = str1[window_start]
      window_start += 1
      if left_char in char_frequency:
        if char_frequency[left_char] == 0:
          matched -= 1  # before putting the character back, decrement the matched count
        char_frequency[left_char] += 1  # put the character back
  return False
```
Time Complexity: O(N+M)
Space Complexity: O(M)

<p align="right"><a href="#top">Back to top</a></p>

## 9. String Anagrams (hard)
Given a string and a pattern, find all anagrams of the pattern in the given string.
```python
def find_string_anagrams(str1, pattern):
  result_indexes = []
  window_start, matched = 0, 0
  char_frequency = {}

  for chr in pattern:
    if chr not in char_frequency:
      char_frequency[chr] = 0
    char_frequency[chr] += 1

  # our goal is to match all the characters from the 'char_frequency' with the current window
  # try to extend the range [window_start, window_end]
  for window_end in range(len(str1)):
    right_char = str1[window_end]
    if right_char in char_frequency:
      # decrement the frequency of matched character
      char_frequency[right_char] -= 1
      if char_frequency[right_char] == 0:
        matched += 1

    if matched == len(char_frequency):  # have we found an anagram?
      result_indexes.append(window_start)

    # shrink the sliding window
    if window_end >= len(pattern) - 1:
      left_char = str1[window_start]
      window_start += 1
      if left_char in char_frequency:
        if char_frequency[left_char] == 0:
          matched -= 1  # before putting the character back, decrement the matched count
        char_frequency[left_char] += 1  # put the character back

  return result_indexes
```
Time Complexity: O(N+M)
Space Complexity: O(M)

<p align="right"><a href="#top">Back to top</a></p>

## 10. Smallest Window containing Substring (hard)
Given a string and a pattern, find the smallest substring in the given string which has all the characters of the given pattern.
```python
def find_substring(str1, pattern):
  window_start, matched, substr_start = 0, 0, 0
  min_length = len(str1) + 1
  char_frequency = {}

  for chr in pattern:
    if chr not in char_frequency:
      char_frequency[chr] = 0
    char_frequency[chr] += 1

  # try to extend the range [window_start, window_end]
  for window_end in range(len(str1)):
    right_char = str1[window_end]
    if right_char in char_frequency:
      char_frequency[right_char] -= 1
      if char_frequency[right_char] >= 0:  # Count every matching of a character
        matched += 1

    # Shrink the window if we can, finish as soon as we remove a matched character
    while matched == len(pattern):
      if min_length > window_end - window_start + 1:
        min_length = window_end - window_start + 1
        substr_start = window_start

      left_char = str1[window_start]
      window_start += 1
      if left_char in char_frequency:
        # Note that we could have redundant matching characters, therefore we'll decrement the
        # matched count only when a useful occurrence of a matched character is going out of the window
        if char_frequency[left_char] == 0:
          matched -= 1
        char_frequency[left_char] += 1

  if min_length > len(str1):
    return ""
  return str1[substr_start:substr_start + min_length]
```
Time Complexity: O(N+M)
Space Complexity: O(M)

<p align="right"><a href="#top">Back to top</a></p>
