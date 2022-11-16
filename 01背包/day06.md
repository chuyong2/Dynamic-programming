# leetcode494. 目标和
## 题目描述
给你一个整数数组 nums 和一个整数 target 。
向数组中的每个整数前添加 '+' 或 '-' ，然后串联起所有整数，可以构造一个 表达式
 - 例如，nums = [2, 1] ，可以在 2 之前添加 '+' ，在 1 之前添加 '-' ，然后串联起来得到表达式 "+2-1" 。返回可以通过上述方法构造的、运算结果等于 target 的不同表达式的数目。
 ## 示例
 ```
示例 1：
输入：nums = [1,1,1,1,1], target = 3
输出：5
解释：一共有 5 种方法让最终目标和为 3 。
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
```
```
示例 2：
输入：nums = [1], target = 1
输出：1
```
我们可以把nums数组分为两部分，一部分是添加+号的，一部分是添加负号的，我们记左边+一堆为left，右边-为right = sum - left（sum为数组元素之和），那么target = 
（正号堆） - （负号堆） = left - sum + left = 2left - sum,此时left = (target + sum)/2(向下取整),这题看target+sum要是非负且偶数。同时sum也要大于target的绝对值
本题是装满有⼏种⽅法。其实这就是⼀个组合问题了
### dp[j] 表⽰：填满j这么⼤容积的包，有dp[i]种⽅法
### 推导
有哪些来源可以推出dp[j]呢？
不考虑nums[i]的情况下，填满容量为j - nums[i]的背包，有dp[j - nums[i]]种方法。
那么只要搞到nums[i]的话，凑成dp[j]就有dp[j - nums[i]] 种方法。举个例子例如：dp[j]，j 为5，
 - 1.已经有一个1（nums[i]） 的话，有 dp[4]种方法 凑成 dp[5]。
 - 2.已经有一个2（nums[i]） 的话，有 dp[3]种方法 凑成 dp[5]。
 - 3.已经有一个3（nums[i]） 的话，有 dp[2]中方法 凑成 dp[5]
 - 4.已经有一个4（nums[i]） 的话，有 dp[1]中方法 凑成 dp[5]
 - 5.已经有一个5 （nums[i]）的话，有 dp[0]中方法 凑成 dp[5]
 那么凑整dp[5]有多少方法呢，也就是把 所有的 dp[j - nums[i]] 累加起来。所以求组合类问题的公式，都是类似这种：
 ```
dp[j] += dp[j - nums[i]]
```
### dp数组如何初始化
从递归公式可以看出，在初始化的时候dp[0] 一定要初始化为1，因为dp[0]是在公式中一切递推结果的起源，如果dp[0]是0的话，递归结果将都是0。
dp[0] = 1，理论上也很好解释，装满容量为0的背包，有1种方法，就是装0件物品。
dp[j]其他下标对应的数值应该初始化为0，从递归公式也可以看出，dp[j]要保证是0的初始值，才能正确的由dp[j - nums[i]]推导出来。
### 遍历
遍历还是一样的套路

```
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int sum = 0;
        for (int i = 0;i<nums.length;i++) {
            sum += nums[i];
        }
        int diff = sum + target;
        if (sum < Math.abs(target) || diff % 2 != 0) {
            return 0;
        }
        int bagSize = diff / 2;
        int[] dp = new int[bagSize + 1];
        dp[0] = 1;
        for (int i = 0;i<nums.length;i++) {
            for (int j = bagSize; j >= nums[i]; j--) {
                dp[j] += dp[j - nums[i]];
            }
        }
        return dp[bagSize];
    }
}
```



