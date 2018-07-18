---
title: leetcode 编码记录
categories: coding
tags:
    - leetcode
    - coding
    - algorithm
---
# leetCode 学习笔记 #
> 一刷 leetcode，因此只要通过就是完成。等待二刷完善和改进代码！！！！！

## 数组 ##

### 从排序数组中删除重复项 ###
给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

**示例 1:**
```
    给定数组 nums = [1,1,2], 
    
    函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 
    
    你不需要考虑数组中超出新长度后面的元素。
```
**示例 2:**
```
    给定 nums = [0,0,1,1,1,2,2,3,3,4],
    
    函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。
    
    你不需要考虑数组中超出新长度后面的元素。
```
**说明:**

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以“引用”方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:
```
    // nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
    int len = removeDuplicates(nums);
    
    // 在函数里修改输入数组对于调用者是可见的。
    // 根据你的函数返回的长度, 它会打印出数组中该长度范围内的所有元素。
    for (int i = 0; i < len; i++) {
    print(nums[i]);
    }
```
> 分析：

** 方案1 ** 
```Python
class Solution:
    def removeDuplicates(self, nums):
        if len(nums) < 2:
            return len(nums)

        lastDiffIndex = 0
        cur = 1
        while cur < len(nums):
            if nums[lastDiffIndex] != nums[cur]:
                lastDiffIndex += 1
                nums[lastDiffIndex] = nums[cur]
                cur += 1
            else:
                cur += 1

        return lastDiffIndex + 1
}
```

s代表**start**, e代表着**end**，我们算法就是如果start和end对应的数字相同时，end继续前进。如果start和end的数字不同，start + 1 替换成end的数字，end继续前进。

```
step1:
    1 1 1 2 2 4 5
    s e
step2:
    1 1 1 2 2 4 5
    s   e
step3:
    1 1 1 2 2 4 5   ---->   1 2 1 2 2 4 5
    s     e                   s   e
step4:
    1 2 1 2 2 4 5
      s     e
step5:
    1 2 1 2 2 4 5  ---->    1 2 4 2 2 4 5
      s       e                 s     e
step5:
    1 2 4 2 2 4 5  ---->    1 2 4 5 2 4 5
        s       e                 s     e
    result is length is the index of s.
```

----------
### 买卖股票的最佳时机 II ###
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。
设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。
注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
**示例 1:**
```
输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
```
**示例 2:**
```
输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```
**示例 3:**
```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```
> 分析：


**方案1**
```Python
class Solution:
    def maxProfit(self, prices):
        
        startIdx = 0
        end = 1
        sum = 0
        priceLen = len(prices)

        if priceLen == 1:
            return 0

        while end < priceLen:
            if prices[startIdx] < prices[end]:
                if end + 1 < priceLen and prices[end] > prices[end + 1]:
                    sum += prices[end] - prices[startIdx]
                    startIdx = end + 1
                    end = startIdx + 1
                else:
                    end += 1
            else:
                startIdx += 1
                end += 1

        if startIdx < priceLen - 1 and prices[startIdx] < prices[priceLen - 1]:
            sum += prices[priceLen - 1] - prices[startIdx]
        return sum
```
解析：符合贪心算法，最大子区间的累加，当出现转折点时，股票卖出

**方案2**

```Python
class Solution:
    def maxProfit(self, nums):
        sum = 0
        nums_len = len(nums)

        if nums_len < 2:
            return sum
        
        for i in range(nums_len - 1):
           if nums[i] < nums[i + 1]:
               sum += nums[i + 1] - nums[i]
        return sum
```
解析：其实思路相同    x1,x2,x3,x4 -->  x4 - x1 == (x2 - x1) + (x3 - x2) + (x4 - x3)

----------
### 旋转数组 ###
给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。
**
示例 1:**
```
输入: [1,2,3,4,5,6,7] 和 k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]
```
**示例 2:**
```
输入: [-1,-100,3,99] 和 k = 2
输出: [3,99,-1,-100]
解释: 
向右旋转 1 步: [99,-1,-100,3]
向右旋转 2 步: [3,99,-1,-100]
```
**说明:**

> 尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
> 要求使用空间复杂度为 O(1) 的原地算法。

**方案1**

```Python
class Solution:
    def rotate(self, nums, k):
        nums_len = len(nums)
        nums[:] =  nums[nums_len - k:] + nums[:nums_len-k]
```

解析：其实并没有运用到任何算法，只是用到了Python的切片来完成

----------

### 存在重复 ###
给定一个整数数组，判断是否存在重复元素。

如果任何值在数组中出现至少两次，函数返回 true。如果数组中每个元素都不相同，则返回 false。

**示例 1:**
```
输入: [1,2,3,1]
输出: true
```
**示例 2:**
```
输入: [1,2,3,4]
输出: false
```
**示例 3:**
```
输入: [1,1,1,3,3,4,3,2,4,2]
输出: true
```

**方案1**

```Python
class Solution:
    def containsDuplicate(self, nums):
        maps = {}
        for num in nums:
            if maps.get(num) == None:
                return False
            else:
               maps[num] = num
        return True      
```
### 加一 

给定一个**非负整数**组成的**非空**数组，在该数的基础上加一，返回一个新的数组。

最高位数字存放在数组的首位， 数组中每个元素只存储一个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

**示例 1:**

```
输入: [1,2,3]
输出: [1,2,4]
解释: 输入数组表示数字 123。
```

**示例 2:**

```
输入: [4,3,2,1]
输出: [4,3,2,2]
解释: 输入数组表示数字 4321。
```

**方案1**

```python
class Solution:
    def plusOne(self, digits):
        digits.reverse()
        divisor = 1
        for index, digit in enumerate(digits):
            digits[index] = (digit + divisor) % 10
            divisor = (digit + divisor) // 10
        
        if divisor == 1:
            digits.append(divisor)

        digits.reverse()
        return digits
```

首先考虑到就是最高为有可能会进1， 所以先把数组**反转**之后循环计算，最后如果最高为是**9**并且补1，那么只要在数组最后添加1就满足。最最后，只要把数组**反转**回来就大功告成。

## 字符串

### 反转字符串

请编写一个函数，其功能是将输入的字符串反转过来。

**示例：**

```
输入：s = "hello"
返回："olleh"
```

**方案1**

```python
class Solution:
    def reverseString(self, s):
        s_array = list(s)
        for i in range((len(s_array) // 2)):
            endInx = len(s_array) - i - 1
            s_array[i], s_array[endInx] = s_array[endInx], s_array[i]
        return ''.join(s_array)
```

思路: 头尾交换