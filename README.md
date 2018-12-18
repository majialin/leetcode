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

### 8. String to Integer (atoi)
Implement `atoi` to convert a string to an integer.

**Hint:** Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.

**Notes:** It is intended for this problem to be specified vaguely (ie, no given input specs). You are responsible to gather all the input requirements up front.

**Requirements for atoi:**

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned. If the correct value is out of the range of representable values, INT_MAX (2147483647) or INT_MIN (-2147483648) is returned.

Anser:
```python
class Solution:
    def myAtoi(self, s):
        """
        :type s: str
        :rtype: int
        """
        s = s.strip()
        for i,v in enumerate(s[1:], 1):
            if v not in '0123456789':
                s = s[:i]
                break
        try:
            s = int(s)
        except ValueError:
            s = 0
        
        s = max(min(2147483647, s), -2147483648)  # just for INT_MAX and INT_MIN requirements in leetcode test
        
        return s
```
Achievement:  
**Your runtime beats 100.00 % of python3 submissions.** Runtime: 64 ms

### 9. Palindrome Number
Determine whether an integer is a palindrome. Do this without extra space.  
**Some hints:**
Could negative integers be palindromes? (ie, -1)

If you are thinking of converting the integer to string, note the restriction of using extra space.

You could also try reversing an integer. However, if you have solved the problem "Reverse Integer", you know that the reversed integer might overflow. How would you handle such case?

There is a more generic way of solving this problem.

Answer:
```python
class Solution:
    def isPalindrome(self, x):
        """
        :type x: int
        :rtype: bool
        """
        y = x
        rev_x = 0
        while y > 0:  # this only reverse positive integer
            rev_x = 10 * rev_x + y%10
            y = y//10
        return True if rev_x==x else False
```
Achievement:  
**Your runtime beats 100.00 % of python3 submissions.** Runtime: 312 ms

### 10. Regular Expression Matching

Implement regular expression matching with support for '.' and '*'.
```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true
```
Answer:
```python
# recursive solution
class Solution:
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
        if p=='':
            if s=='':
                return True
            else:
                return False
        
        try:
            if p[1]=='*':
                num1 = '*' # num1 means the the number of first charactor in p
            else:
                num1 = '1'
        except IndexError:
            num1 = '1'
        
        if s=='':
            if num1=='*':
                return self.isMatch(s,p[2:])
            return False
            
        if num1 == '*':
            if p[0]=='.' or p[0]==s[0]:
                return self.isMatch(s[1:], p) or self.isMatch(s, p[2:])
            else:
                return self.isMatch(s, p[2:])
        
        if p[0]=='.' or p[0]==s[0]:
            return self.isMatch(s[1:], p[1:])
        return False
```
```python
# dynamic programming solution
class Solution:
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
        temp = {}
        # define m(i,j) as " if s[i:] match p[j:] "
        def m(i,j):
            if (i,j) in temp:
                return temp[i,j]
            else:
                if j>len(p)-1:
                    ans = i>len(s)-1
                else:
                    first_match = i<=len(s)-1 and p[j] in s[i]+'.'
                    if j<len(p)-1 and p[j+1]=='*':
                        ans =  (first_match and m(i+1,j)) or m(i,j+2)
                    else:
                        ans = first_match and m(i+1,j+1)
                temp[i,j] = ans
                return ans
        return m(0,0)
```

### 15. 3Sum
Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:

The solution set must not contain duplicate triplets.

Example:
```
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```
Answer:
```python
# Change to a 2Sum problem
class Solution:
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        nums.sort()
        result = []
        for i,num1 in enumerate(nums): # 2Sum solution with dict-map
            if i>0 and num1==nums[i-1]:
                continue
            sumof2 = 0-num1
            d = {}
            nums2 = nums[i+1:]
            j_temp = None
            for j,num2 in enumerate(nums2):
                if num2 in d:
                    if d[num2]==j_temp:
                        continue
                    result.append([num1,nums2[d[num2]],num2])
                    j_temp = d[num2]
                else:
                    d[sumof2-num2] = j
        return result
```

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
