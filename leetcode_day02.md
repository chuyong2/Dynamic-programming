# 刷题第二日
## leetcode746. 使用最小花费爬楼梯
给你一个整数数组 cost ，其中 cost[i] 是从楼梯第 i 个台阶向上爬需要支付的费用。一旦你支付此费用，即可选择向上爬一个或者两个台阶。
你可以选择从下标为 0 或下标为 1 的台阶开始爬楼梯。请你计算并返回达到楼梯顶部的最低花费。

这题刚开始我看到是懵的，因为没看懂题目，花了大概三分钟看懂了，就是上来可以选择下标为0 | 1的台阶开始爬，然后支付该下表位置的费用可以再爬一到二个台阶，直到爬完，
这里的爬到楼顶指的是下标最后一个的下一位，例如 [10，15，20]我在20后面加入0[10,15,20,0]爬到0即可。
 - dp数组，dp[i]只爬到第i个台阶花费最少的体力
 - 确定递推公式:dp[i-1]上一个台阶到dp[i],花费cost[i-1];dp[i-2]上两个台阶到dp[i],花费cost[i-2]。那么递推公式为：dp[i]=min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])
 - 初始化：上来选0或1，不用花费，即dp[0]=dp[1]=0
 - 遍历顺序：从前往后
 
java代码如下：
```
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int[] dp = new int[cost.length + 1];
        dp[0] = 0;
        dp[1] = 0;
        for(int i = 2;i<dp.length;i++){
            dp[i] = Math.min(dp[i-1] + cost[i-1],dp[i-2] + cost[i-2]);
        }
        return dp[cost.length];
    }
}
```

## leetcode62 不同路径
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。
问总共有多少条不同的路径？
![image](https://user-images.githubusercontent.com/88364565/197534539-95fdafd2-79c8-42fe-ae41-8adea7006510.png)
 - dp[i][j]表示到i和j的位置有d[i][j]种路径，默认从(0,0)到(m-1，n-1)
 - 递推公式：dp[i][j] = dp[i-1][j] + dp[i][j-1]
 - 初始化：dp[0][j] = 1 ,dp[i][0] = 1;(从(0,0)开始到(i,0)只有一条路径)
 - 遍历顺序：从前往后
 
 java:
 ```
 class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        for(int i = 0;i < m;i++) dp[i][0] = 1;
        for(int j = 0;j < n;j++) dp[0][j] = 1;
        for(int i = 1;i < m;i++){
            for(int j = 1;j < n;j++){
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
}
 ```

## leetcode63 不同路径 II
第三天才发现这是第二题的升级版，也就把他放到第二天了，这一题主要是多了障碍，我们把obstacleGrid[i][j] == 1看作有障碍，那么dp[i][j] = 0
反之obstacleGrid[i][j] == 0,就是没障碍，继续执行dp[i][j] = dp[i-1][j] + dp[i][j-1],本题还需要注意初始化问题，就是如果第一行或者第一列有障碍
我们用第一行有障碍举例，即obstacleGrid[i][0] == 1,那么dp[i][0] = 0,obstacleGrid == 0,dp[i][0] = 1;
java代码如下
```
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        int[][] dp = new int[m][n];
        for(int i = 0;i < m && obstacleGrid[i][0] == 0;i++) dp[i][0] = 1;
        for(int j = 0;j < n && obstacleGrid[0][j] == 0;j++) dp[0][j] = 1;
        for(int i = 1;i < m;i++){
            for(int j= 1;j < n;j++){
                if(obstacleGrid[i][j] == 0){
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
                }
            }
        }
        return dp[m-1][n-1];
    }
}
```













