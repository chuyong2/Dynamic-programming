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
## leetcode96 不同二叉搜索树
给你一个整数 n ，求恰由 n 个节点组成且节点值从 1 到 n 互不相同的 二叉搜索树 有多少种？返回满足题意的二叉搜索树的种数。
![image](https://user-images.githubusercontent.com/88364565/198170169-f2980556-e029-462e-9a43-26d7a9a59dca.png)

这道题首先得直到什么是二叉搜索树，我拿n=3举例，首先以1为根节点，那么2和3只能放在他右边，因为2，3大于1（与根节点相比较，大于根节点放在右边，小于放在左边）

![image](https://user-images.githubusercontent.com/88364565/198170633-9bc279c5-a260-490d-afa4-5b14bd7e25c5.png)、

当根节点为2

![image](https://user-images.githubusercontent.com/88364565/198170941-693c36dc-8ffc-41b7-95ff-2bb6f065860e.png)

当根节点为3

![image](https://user-images.githubusercontent.com/88364565/198170871-96ad674b-5734-48a2-8c33-c271a862ddd7.png)

可以看出当n为3时可以用n=2，n=1，n=0时推导出来也就是dp[3]和dp[2],dp[1]有关，可以想到用动态规划
 - dp[i]表示元素i为不同数作为根节点的二叉搜索树个数
 - 引入j作为根节点，遍历从1-i，左子树节点数量为j-1，右子树节点数量为i-j，所以递推公式为：dp[i] += dp[j-1] * dp[i-j]
 - 初始化dp[0] = 1，一或n作为根节点，其左子树或右子树为空节点，如果为0那么递推公式不成立，我们初始化为1就行
java代码：
```
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n+1];
        dp[0] = 1;
        for(int i = 1 ;i <= n;i++){
            for(int j = 1 ;j <= i ;j++){
                dp[i] += dp[j-1] * dp[i-j];
            }
        }
        return dp[n];
    }
}
//时间复杂度O(n^2)
//空间复杂度O(n)
```






