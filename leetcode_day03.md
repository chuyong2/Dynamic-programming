# 第三天
## leetcode343. 整数拆分
给定一个正整数 n ，将其拆分为 k 个 正整数 的和（ k >= 2 ），并使这些整数的乘积最大化。返回 你可以获得的最大乘积 。
对于正整数n，当n >= 2，可以拆分成至少两个正整数的和。由于每个正整数对应的最大乘积取决于比它小的正整数对应的最大乘积，因此可以使用动态规划求解。
 - 确定dp数组，dp[i]表示拆分为最少两个数字的最大乘积
 - dp[0] = dp[1] = 0，解释：0拆不了，1只能拆分为0和1，故dp[0] = dp[1] = 0
 - 确定公式：我们设数字有一个整数i，拆分的第一个数字为j，如果不可继续拆分，即为j*(i-j)，如果可以继续拆分即为j*dp[i-j]
 公式可以确定：dp[i] = max(j*(i-j),j*dp[i-j])
java代码
```
class Solution {
    public int integerBreak(int n) {
        if(n < 2){
            return 0;
        }
        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 0;
        for(int i =2;i<= n;i++)
        {
            for(int j = 1;j<i ;j++){
                dp[i] = Math.max(dp[i],Math.max(j*(i - j) , j * dp[i-j]));//这里之所以要再取dp[i]和dp[i] = max(j*(i-j),j*dp[i-j])的最大值是数组遍历过程中值会覆盖
            }
        }
        return dp[n];
    }
}
```
