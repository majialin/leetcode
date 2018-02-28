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
### 5. Longest Palindromic Substring
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

Example:
```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
 ```
Example:
```
Input: "cbbd"
Output: "bb"
```
Answer:
```python
class Solution:
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """
        palindromic_list = []
        for i in range(len(s)):
            l, r = i-1, i
            palindromic_str = ''
            while l>=0 and r<len(s):  # this cycle result longest palindrome with even length
                if s[l] == s[r]:
                    palindromic_str = s[l:r+1]
                else:
                    break
                l, r = l-1, r+1
            palindromic_list.append(palindromic_str)
            
            l = r = i
            palindromic_str = ''
            while l>=0 and r<len(s):  # this cycle result longest palindrome with odd length
                if s[l] == s[r]:
                    palindromic_str = s[l:r+1]
                else:
                    break
                l, r = l-1, r+1
            palindromic_list.append(palindromic_str)
            
        return max(palindromic_list, key=len, default='')
```

### 6. ZigZag Conversion

The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)
```
P   A   H   N
A P L S I I G
Y   I   R
```
And then read line by line: `"PAHNAPLSIIGYIR"`
Write the code that will take a string and make this conversion given a number of rows:
```
string convert(string text, int nRows);
```
`convert("PAYPALISHIRING", 3)` should return `"PAHNAPLSIIGYIR"`.

Anwser:
```python
class Solution:
    def convert(self, s, numRows):
        """
        :type s: str
        :type numRows: int
        :rtype: str
        """
        if numRows == 1:
            return s
        martrix = []
        
        x, y = divmod(len(s), 2*numRows-2)
        if y == 0:
            numCols = x*2
        else:
            numCols = x*2 + (1 if y<= numRows else 2)    # how many columns
        
        for n in range(numCols):                      # n means column number
            col = []
            if n%2 == 0:                                         # when comes to even column
                l = n//2 * numRows + n//2 * (numRows-2)        # start index of s in column
                r = l + numRows
                for i in range(l,r):
                    col.append(s[i] if i<len(s) else '')
            else:
                l = (n//2 + 1) * numRows + n//2 * (numRows-2)
                r = l + numRows - 2
                for i in range(l,r):
                    col.append(s[i] if i<len(s) else '')
                col.insert(0,'')
                col.append('')
                col.reverse()
            martrix.append(col)

        # martrix[col1,col2,col3...]
        martrix = [[row[i] for row in martrix] for i in range(numRows)]  # transpose
        letter_list = [letter for row in martrix for letter in row]
        return ''.join(letter_list)
```

### 7. Reverse Integer
Given a 32-bit signed integer, reverse digits of an integer.

Example 1:
```
Input: 123
Output:  321
```
Example 2:
```
Input: -123
Output: -321
```
Example 3:
```
Input: 120
Output: 21
```
Note:
Assume we are dealing with an environment which could only hold integers within the 32-bit signed integer range. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

Answer:
```python
class Solution:
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        positive = True if x>=0 else False
        r = int(str(abs(x))[::-1])
        r = r if positive else -r
        
        # only for leetcode test
        if r > 2147483647 or r < -2147483648:
            return 0
        
        return r
```
Notice:  
Another answer is numeric solution like x%10. (a basic problem which i encoutered in college)

Achievement:  
**Your runtime beats 100.00 % of python3 submissions.** Runtime: 60 ms

### 307. Range Sum Query - Mutable
Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

The update(i, val) function modifies nums by updating the element at index i to val.
Example:
```
Given nums = [1, 3, 5]

sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8
```
Note:
1. The array is only modifiable by the update function.
2. You may assume the number of calls to update and sumRange function is distributed evenly.

Answer:
```python
class NumArray:

    def __init__(self, nums):
        """
        :type nums: List[int]
        """
        self.nums = nums
        

    def update(self, i, val):
        """
        :type i: int
        :type val: int
        :rtype: void
        """
        self.nums[i] = val
        

    def sumRange(self, i, j):
        """
        :type i: int
        :type j: int
        :rtype: int
        """
        return sum(self.nums[i:j+1])
        


# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(i,val)
# param_2 = obj.sumRange(i,j)
```
### 29. Divide Two Integers
Divide two integers without using multiplication, division and mod operator.

If it is overflow, return MAX_INT.

Answer:
```python
class Solution:
    def divide(self, dividend, divisor):
        """
        :type dividend: int
        :type divisor: int
        :rtype: int
        """
        d = abs(dividend)
        a = c = abs(divisor)
        r = 0
        i = 0
        while a <= d:
            a <<= 1
            i += 1
        while i>0:
            i -= 1
            a = c<<i
            if d>=a:
                d -= a
                r = r + (1<<i) # https://docs.python.org/3/reference/expressions.html#operator-precedence

        if (dividend<0 and divisor>0) or (dividend>0 and divisor<0):
            return -r
        if r > 2147483647:
            return 2147483647    
        return r
```
Notice:  
Python3 doesn't have max_int problem, `if r > 2147483647:` is just for leetcode test cases.

Achievement:  
**Your runtime beats 100.00 % of python3 submissions.** Runtime: 60 ms
