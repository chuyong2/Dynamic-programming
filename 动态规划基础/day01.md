今天算是第一次正式刷题，以前其实也有零零散散的刷题，但是都没有一个周期性的过程，而且能自己写出来的题也不多，可以说是一个手也能数过来，刷题呢也是提升自己数据结构和算法的能力，
我也是推荐大家按模块刷，上来每日一题可以看看，不必深究，每日一到三题（当然也看题目难度吧），我也是新手（包括写博客），大概半个月前刷了五六题的贪心，然后上课太忙给忘了。
今天就从动态规划开始吧。
首先，动态规划（Dynamic Programming），我们称dp就行，如果一个问题有很多子问题是重叠的，就可以用动态规划。
## 动态规划五步骤：
1. 确定dp数组以及下标的含义
2. 确定递推公式
3. dp数组如何初始化
4. 确定遍历顺序
5. 举例推导dp数组
## 那么先从大佬推荐的第一题入手（leetcode 509 斐波那契数列）
斐波那契数 （通常用 F(n) 表示）形成的序列称为 斐波那契数列 。该数列由 0 和 1 开始，后面的每一项数字都是前面两项数字的和。也就是：F(0) = 0，F(1) = 1
  F(n) = F(n - 1) + F(n - 2)，其中 n > 1，给定 n ，请计算 F(n) 。
相信这种题目大家都不陌生，大学里很喜欢考的题目，尤其是c语言这门课，当时也用c语言看过几题力扣，很复杂，之后学了java确实简单了不少，这个题首先是想到递归，递归很好实现
斐波那契数列,代码如下：
```
class Solution {
    public int fib(int n) {
        if(n == 0 || n == 1){
            return n;
        }
         return fib(n - 1) + fib(n - 2);
    }
}
```
当然大家也都知道，递归效率是很低的，解决问题虽然很简单，缺点也很明显，所以我们得考虑效率更高的方法，那就是动态规划
按照代码随想录编写老师的方法来讲我们分为五个步骤：
1.dp数组为F，dp[i]为F[i],表示第i个数的斐波那契数列值
2.递推公式dp[i] = dp[i - 1] + dp[i - 2]
3.初始化:dp[0] = 0,dp[1] = 1
4.遍历顺序:从前往后
5.推导过程：我们取n=15  0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610
```
class Solution {
    public int fib(int n) {
        if(n == 0 || n == 1){
            return n;
        }
       int[] dp = new int[n+1];
       dp[0] = 0;
       dp[1] = 1;
       for(int i = 2;i <= n;i++){
           dp[i] = dp[i - 1] + dp[i - 2];
       }
       return dp[n];
    }
}
```
```
//不用数组
class Solution {
    public int fib(int n) {
        if(n == 0 || n == 1){
            return n;
        }
       int a = 0, b=1 , c=0;
       for(int i = 2;i <= n;i++){
           c = a + b;
           a = b;
           b = c;
       }
       return c;
    }
}
```
## 来到第二题 leetcode70 爬楼梯
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
分析一下题目（我举的例子，感觉跟数学归纳法一样） 第n级台阶 i=1，2，3，4，5  方法数sum=1，2，3，5，8
公式就能推导出来为：f(n) = f(n-1) + f(n-2)，f(i)表示爬到了第i楼，初始化f(1)=1,f(2)=2,题目给范围是大于等于1的，所以我没有考虑n=0的情况
```
class Solution {
    public int climbStairs(int n) {
        if(n == 1){
            return 1;
        }
        int[] dp = new int[n+1];
        dp[1] = 1;
        dp[2] = 2;
        for(int i = 3;i <= n;i++){
            dp[i] = dp[i-1] + dp[i-2];
        }
        return dp[n];
    }
}
```
优化空间复杂度
```
class Solution {
    public int climbStairs(int n) {
        if(n == 1){
            return 1;
        }
        int first = 1;
        int second = 2;
        for(int i = 3;i <= n;i++){
            int third = first + second;
            first = second;
            second = third;
        }
        return second;
    }
}
```
