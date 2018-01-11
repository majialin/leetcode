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
