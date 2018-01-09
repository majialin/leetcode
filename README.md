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
