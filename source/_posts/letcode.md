---
title: leetcode 编码记录
categories: coding
tags:
    - leetcode
    - coding
    - algorithm
---
# leetCode 学习笔记 #
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

### 方案1 （超出时间限制） ###
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

```Javascript
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
```Javascript
输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
```
**示例 2:**
```Javascript
输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```
**示例 3:**
```Javascript
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```
> 分析：

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
