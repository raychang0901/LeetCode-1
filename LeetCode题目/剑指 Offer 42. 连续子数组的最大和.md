# 题目[面试题42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

输入一个整型数组，数组里有正数也有负数。数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。

 

示例1:

```
输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```



*****

# Python解题思路

## 方法1：动态规划

这题其实做过了，用动态规划的思路可以解决，用一个变量记录最大值，用一个变量记录累计，如果累计的结果小于0就赋予0进行初始化。

至于为什么小于0就要赋予0呢？原因很简单，如果加完这个数都是负的话，不管下一次加什么数，都会比这次的值小。所以果断放弃那些相加后会小于0的记录。

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        tem_max, MAX = 0, float("-inf")
        for each in nums:
            tem_max += each
            MAX = max(MAX, tem_max)
            if tem_max < 0: tem_max = 0
        return MAX
```

运行结果

```
执行用时 :64 ms, 在所有 Python3 提交中击败了92.87% 的用户
内存消耗 :17.5 MB, 在所有 Python3 提交中击败了100.00%的用户

执行用时 :72 ms, 在所有 Python3 提交中击败了86.05% 的用户
内存消耗 :17.5 MB, 在所有 Python3 提交中击败了100.00%的用户

执行用时 :72 ms, 在所有 Python3 提交中击败了86.05% 的用户
内存消耗 :17.4 MB, 在所有 Python3 提交中击败了100.00%的用户
```

## 方法2：动态规划---极简版

看了大神的[解题](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/solution/mian-shi-ti-42-lian-xu-zi-shu-zu-de-zui-da-he-do-2/)，妙啊.

这里而外空间也不用，直接修改原数组，原数组从第二项开始，只有前一项是大于0的才相加(其实原理是一样的，只是下面的算法更简约)

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        for index in range(1, len(nums)):
            nums[index] += nums[index-1] if nums[index-1] > 0 else 0
        return max(nums)
```

运行结果

```
执行用时 :68 ms, 在所有 Python3 提交中击败了90.21% 的用户
内存消耗 :18.2 MB, 在所有 Python3 提交中击败了100.00%的用户

执行用时 :72 ms, 在所有 Python3 提交中击败了86.05% 的用户
内存消耗 :18.3 MB, 在所有 Python3 提交中击败了100.00%的用户

执行用时 :100 ms, 在所有 Python3 提交中击败了38.63% 的用户
内存消耗 :18.3 MB, 在所有 Python3 提交中击败了100.00%的用户
```

欢迎来github上看更多题目的解答[力扣解题思路](https://github.com/WRAllen/LeetCode)

  