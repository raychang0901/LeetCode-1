# 题目[面试题26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

例如:
给定的树 A:

```
     3
    / \

   4   5
  / \
 1   2
给定的树 B：

   4 
  /
 1
返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。
```



示例 1：

```
输入：A = [1,2,3], B = [3,1]
输出：false
```



示例 2：

```
输入：A = [3,4,5,1,2], B = [4,1]
输出：true
```

限制：

0 <= 节点个数 <= 10000

PS:通过B和A的所有root的ID比较，发现B是一课新的树

*****

# Python解题思路

看题目给的示例条件，发现数只是一棵普普通通的二叉树，也就是说可能会有重复的结点

那我想到的就是通过，前序或者中序或者后序遍历的方法同时遍历AB树，然后判断B树的结果是否包含在A里面。估摸这个方法效率很差, 而且遇到一个问题，我要如何判断[4,1,4,2,3]和[4,2]。

感觉这样即麻烦出问题的概率又大（不要问为什么，因为我测试过了）

## 方法1：递归判断---错误代码

那换种思路，就是直接拿A与B进行比较，假设我们的B是从A的root开始的，那问题就简单了，我只有判断递归的left以及递归的right是否和B左右子树递归的结果相等即可，那要是B不是从A的root开始呢？？？那也简单，我们进行A左右子树的深入即可

```python
class Solution:
    def isSubStructure(self, A: TreeNode, B: TreeNode) -> bool:
        if not A and B: return False
        if (A and not B) or (not A and not B): return True
        if A.val == B.val:
            return self.isSubStructure(A.left, B.left) and self.isSubStructure(A.right, B.right)
        else:
            return self.isSubStructure(A.left, B) or self.isSubStructure(A.right, B)
```

运行结果

```
45 / 47 个通过测试用例
	状态：解答错误
	
提交时间：0 分钟之前
输入： [-1,3,2,0]
[]
输出： true
预期： false
```

卧槽！！！ 这个原因很简单，没进行递归之前就命中了上面的判断（空的B不属于A的子树），那现在问题就来了，我要怎么判断B是递归匹配到空而不是一开始就是空呢？

## 方法2：递归判断---加上flag

于是有了下面的代码

```python
class Solution:
    is_first = True
    def isSubStructure(self, A: TreeNode, B: TreeNode) -> bool:
        if not A and B: return False
        if not self.is_first:
            if (A and not B) or (not A and not B): return True
        else:
            if A and not B: return False
        self.is_first = False
        if A.val == B.val:
            return self.isSubStructure(A.left, B.left) and self.isSubStructure(A.right, B.right)
        else:
            return self.isSubStructure(A.left, B) or self.isSubStructure(A.right, B)
```

运行结果

```
46 / 47 个通过测试用例
	状态：解答错误
	
提交时间：0 分钟之前
输入： [1,0,1,-4,-3]
[1,-4]
输出： true
预期： false
```

卧槽，自以为可以通过测试的，尽然最后一个测试用例没有过？？？

仔细看了一遍代码发现了逻辑上的重大问题 当A的root和B的root不匹配的时候else语句里面会忽略掉当前的匹配情况

就如这最后一个测试用例一样，A和B的1以及匹配了，当是下一层A的 0 和 1 都不匹配B的 -4 进入了else, 最后在A0的左子树匹配到了B的-4， 所以输出了True。于是机智的我又加了一个flag进去（诶呀，感觉是非常糟糕的代码风格了现在）

```python
class Solution:
    is_first = True
    is_start = False
    def isSubStructure(self, A: TreeNode, B: TreeNode) -> bool:
        if not A and B: return False
        if not self.is_first:
            if (A and not B) or (not A and not B): return True
        else:
            if A and not B: return False
        self.is_first = False
        if self.is_start:
            if A.val == B.val:
                self.is_start = True
                return self.isSubStructure(A.left, B.left) and self.isSubStructure(A.right, B.right)
           	# 如果以及开始匹配了遇到子树不匹配的情况直接放回False
            return False
       	# 进入这个else表示B还一个都没有匹配到
        else:
            if A.val == B.val:
                self.is_start = True
                return self.isSubStructure(A.left, B.left) and self.isSubStructure(A.right, B.right)
            else:
                return self.isSubStructure(A.left, B) or self.isSubStructure(A.right, B)
```

运行结果

```
执行用时 :112 ms, 在所有 Python3 提交中击败了84.16% 的用户
内存消耗 :17.9 MB, 在所有 Python3 提交中击败了100.00%的用户

执行用时 :112 ms, 在所有 Python3 提交中击败了84.16% 的用户
内存消耗 :18.2 MB, 在所有 Python3 提交中击败了100.00%的用户

执行用时 :104 ms, 在所有 Python3 提交中击败了96.21% 的用户
内存消耗 :18.2 MB, 在所有 Python3 提交中击败了100.00%的用户
```

虽然通过了，但是我非常不喜欢用flag来解决问题，因为容易出现错误，特别是三个以上并且有相关关系的flag，后期代码维护起来也非常难受

## 方法3：递归判断---不用flag

看了一下大神们的题解，其中有个[解题思路](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/solution/pythonti-jie-si-lu-qing-xi-de-dfs-by-xiao-xue-66/)点醒了我, 就是我可以分开判断

```python
class Solution:
    def isSubStructure(self, A: TreeNode, B: TreeNode) -> bool:
        if (not A and B) or not B or (not A and not B): return False
        if A.val == B.val: return self.is_equal(A, B)
        else: return self.isSubStructure(A.left, B) or self.isSubStructure(A.right, B)
    
    def is_equal(self, subA, subB):
        if not subB: return True
        if not subA: return False
        if subA.val != subB.val: return False
        return self.is_equal(subA.left, subB.left) and self.is_equal(subA.right, subB.right)
```

运行结果

```
执行用时 :112 ms, 在所有 Python3 提交中击败了84.16% 的用户
内存消耗 :18.1 MB, 在所有 Python3 提交中击败了100.00%的用户

执行用时 :112 ms, 在所有 Python3 提交中击败了84.16% 的用户
内存消耗 :18.1 MB, 在所有 Python3 提交中击败了100.00%的用户

执行用时 :116 ms, 在所有 Python3 提交中击败了76.40% 的用户
内存消耗 :18.1 MB, 在所有 Python3 提交中击败了100.00%的用户
```

上面这个代码看着就非常的舒服！！！特别是对比了方法2的代码（能写出可以解决问题并且优雅的代码像吸drug一样，这大概就是python这该死的魅力(￣▽￣)"）

突然想到昨天看Gunicorn源码的时候(随便看看的)发现其中一个作者的注释`# I love dynamic language`哈哈哈

欢迎来github上看更多题目的解答[力扣解题思路](https://github.com/WRAllen/LeetCode)

  