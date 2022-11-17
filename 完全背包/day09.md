# leetcode[377. 组合总和 Ⅳ](https://leetcode.cn/problems/combination-sum-iv/)

## 题目描述

给你一个由 不同 整数组成的数组 nums ，和一个目标整数 target 。请你从 nums 中找出并返回总和为 target 的元素组合的个数。

题目数据保证答案符合 32 位整数范围。

## 示例

```
示例一
输入：nums = [1,2,3], target = 4
输出：7
解释：
所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
请注意，顺序不同的序列被视作不同的组合。
```

```
示例二
输入：nums = [9], target = 3
输出：0
```

此题与day08的leetcode518的差别就是一个要看顺序，一个不看顺序，即排列与组合的差别

动态规划五部曲

- dp[i] 指凑成目标i的排列数为dp[i]
- 确定递推公式 dp[i] += dp[i - nums[j]]
- 初始化dp[0] = 1
-  确定遍历顺序，这是与组合问题不一样的地方，组合问题是先便利物品，再遍历背包，排列问题则相反

java代码

```java
class Solution {

  public int combinationSum4(int[] nums, int target) {
   int[] dp = new int[target + 1];
   dp[0] = 1;
   for(int i = 0;i < target + 1;i++){
      for(int j = 0;j < nums.length;j++){
       if(i - nums[j] >= 0){
         dp[i] += dp[i - nums[j]];
       }
     }
   }
   return dp[target];
 }
}


```
# leetcode70 爬楼梯
## 题目描述
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
## 示例
示例 1：
```
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶
```
示例二
```
输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。
1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
```
回过来再看一下这题才发现是完全背包问题
java代码
```
class Solution {
    public int climbStairs(int n) {
        int[] values = {1,2};
        int[] dp = new int[n + 1];
        dp[0] = 1;
        for(int i = 0;i < n+1;i++){
            for(int j = 0;j < values.length;j++){
                if(i - values[j] >= 0){
                    dp[i] += dp[i - values[j]];
                }
            }
        }
        return dp[n];
    }
}
```
稍微优化，不用数组
```
class Solution {
    public int climbStairs(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 1;
        for(int i = 1;i < n+1; i++){
            for(int j = 1;j <= 2;j++){
                if(i - j >= 0){
                    dp[i] += dp[i - j];
                }
            }
        }
        return dp[n];
    }
}
```

此题其实可以改编为更上一阶的题目，把只能上1或2改为最多上m（1-m）
 - dp[i]：爬到有i个台阶的楼顶，有dp[i]种⽅法
 - dp[i]有⼏种来源，dp[i - 1]，dp[i - 2]，dp[i - 3] 等等，即：dp[i - j]
```
class Solution {
    public int climbStairs(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 1;
        for(int i = 1;i < n+1; i++){
            for(int j = 1;j <= m;j++){
                if(i - j >= 0){
                    dp[i] += dp[i - j];
                }
            }
        }
        return dp[n];
    }
}
```








