# 题目[面试题10- II. 青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

示例 1：

```
输入：n = 2
输出：2
```



示例 2：

```
输入：n = 7
输出：21
```



提示：

    0 <= n <= 100

*****

# Python解题思路

其实这题和[上一题](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)本质上是一样的

其实这题我先想到的不是动态算法，而是递归

## 方法1：递归算法---超时

```python
class Solution:
    def numWays(self, n: int) -> int:
        if n == 0: return 1
        if n == 1: return 1
        if n == 2: return 2
        return (self.numWays(n-1)+self.numWays(n-2)) % 1000000007
```

运行结果

```
20 / 51 个通过测试用例
	状态：超出时间限制
	
提交时间：0 分钟之前
最后执行的输入： 37
```

诶呀超时了！那只能用动态了

## 方法2：动态规划

可以知道青蛙跳台和之前的斐波那契数列是同样的问题,唯一的区别是斐波第一位是0，而青蛙第一位是1

话说为啥0台阶的结果不是0啊

```python
class Solution:
    def numWays(self, n: int) -> int:
        prepre, pre = 1, 1
        for _ in range(n):
            prepre, pre = pre, prepre + pre
        return prepre % 1000000007
```

运行结果

```
执行用时 :44 ms, 在所有 Python3 提交中击败了40.58% 的用户
内存消耗 :13.5 MB, 在所有 Python3 提交中击败了100.00%的用户

执行用时 :52 ms, 在所有 Python3 提交中击败了21.81% 的用户
内存消耗 :13.7 MB, 在所有 Python3 提交中击败了100.00%的用户

执行用时 :36 ms, 在所有 Python3 提交中击败了76.51% 的用户
内存消耗 :13.5 MB, 在所有 Python3 提交中击败了100.00%的用户
```

欢迎来github上看更多题目的解答[力扣解题思路](https://github.com/WRAllen/LeetCode)

  