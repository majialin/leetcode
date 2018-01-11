# leetcode

### 1. Two Sum
Given an array of integers, return indices of the two numbers such that they add up to a specific target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:
```
Given nums = [2, 7, 11, 15], target = 9,
    
Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```
Answer:
```python
class Solution:
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        l = len(nums)
        for i in range(0, l-1):
            dif = target - nums[i]
            for j in range(i+1, l):
                if nums[j] == dif:
                    return [i, j]
# 时间复杂度高，吸取教训。
# dict 比 list 查找快（因为hash索引）
# https://wiki.python.org/moin/TimeComplexity
```
Answer2:
```python
class Solution:
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        dic = {}
        for index, num in enumerate(nums):
            if num in dic:
                return [dic[num], index]
            dic[target - num] = index
# 鉴自他人
```

### 2. Add Two Numbers
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Example
```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```
Answer:
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        sig1 = sig2 = True
        val = l1.val + l2.val
        l3 = l4 = ListNode(val%10)
        while sig1 or sig2: # 两个链表都没数据时，再停止
            l1 = l1.next
            l2 = l2.next
            if l1 == None:
                sig1 = False # sig 为False时，l1数据已结尾
                l1 = ListNode(0)
            if l2 == None:
                sig2 = False
                l2 = ListNode(0)
            val = l1.val + l2.val + val//10
            if val == 0 and (not sig1) and (not sig2): # 最后一个进位为0时，跳过添加节点
                break
            l3.next = ListNode(val%10)
            l3 = l3.next
        return l4
```
### 3. Longest Substring Without Repeating Characters
Given a string, find the length of the longest substring without repeating characters.

Examples:

Given "abcabcbb", the answer is "abc", which the length is 3.

Given "bbbbb", the answer is "b", with the length of 1.

Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
Answer:
```python
class Solution:
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        List_of_len = []
        long_sub = ''
        for i in s:
            if i in long_sub:
                List_of_len.append(len(long_sub))
                long_sub = long_sub.split(i)[1]
            long_sub = long_sub + i
        List_of_len.append(len(long_sub))
        return max(List_of_len)
```
### 4. Median of Two Sorted Arrays
There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

Example 1:
```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```
Example 2:
```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```
Answer:
```python
class Solution:
    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        nums = sorted((nums1 + nums2))
        x, y = divmod(len(nums), 2)
        if y:
            return nums[x] * 1.0
        else:
            return (nums[x-1] + nums[x])/2 * 1.0
```
