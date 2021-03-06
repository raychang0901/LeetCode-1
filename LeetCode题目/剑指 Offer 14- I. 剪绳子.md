# 题目[面试题14- I. 剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m] 。请问 k[0]*k[1]*...*k[m] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

示例 1：

```
输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1
```



示例 2:

```
输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
```



提示：

    2 <= n <= 58

*****

# Python解题思路

想了半天没想出结果，求助一下答案

## 方法1：动态规划

看了一遍代码，好似理解了，这边解释一下：核心思路就是穷举割绳子的时候只割一次，也就是只把绳子分为两个部分，至于这两个部分到底之前割了几次不管（之前的最大值）

```python
class Solution:
    def cuttingRope(self, n: int) -> int:
        # 初始化-由于下标的原因又要取到n所以+1
        dp = [1]*(n+1)
        # 外层遍历获取之前的动态结果，目的是为了获取n的动态结果
        for each_n in range(2, n+1):
            # 内层循环用来割绳子，从1割到each_n，目的是获取割一遍绳子来获取最大值
            # 也就是穷举出所有可能
            for cut_len in range(1, each_n):
                # 这里用cut_len * dp[each_n-cut_len] 或者 dp[cut_len] * (each_n-cut_len)是一样的
                # 为啥二者都行？其实很简单，cut_len和each_n-cut_len是互补的，循环一遍后都取到了
                dp[each_n] = max(cut_len*dp[each_n-cut_len], cut_len*(each_n-cut_len), dp[each_n])
        return dp[n]
```

运行结果

```
执行用时 :52 ms, 在所有 Python3 提交中击败了30.43% 的用户
内存消耗 :13.7 MB, 在所有 Python3 提交中击败了100.00%的用户

执行用时 :44 ms, 在所有 Python3 提交中击败了57.49% 的用户
内存消耗 :13.5 MB, 在所有 Python3 提交中击败了100.00%的用户

执行用时 :72 ms, 在所有 Python3 提交中击败了8.53% 的用户
内存消耗 :13.7 MB, 在所有 Python3 提交中击败了100.00%的用户
```

欢迎来github上看更多题目的解答[力扣解题思路](https://github.com/WRAllen/LeetCode)

  