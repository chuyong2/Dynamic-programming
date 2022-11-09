# leetcode 474. 一和零
## 题目描述
给你一个二进制字符串数组 strs 和两个整数 m 和 n 。请你找出并返回 strs 的最大子集的长度，该子集中 最多 有 m 个 0 和 n 个 1 。
如果 x 的所有元素也是 y 的元素，集合 x 是集合 y 的 子集 。

## 示例
```
示例 1：
输入：strs = ["10", "0001", "111001", "1", "0"], m = 5, n = 3
输出：4
解释：最多有 5 个 0 和 3 个 1 的最大子集是 {"10","0001","1","0"} ，因此答案是 4 。
其他满足题意但较小的子集包括 {"0001","1"} 和 {"10","1","0"} 。{"111001"} 不满足题意，因为它含 4 个 1 ，大于 n 的值 3 。
```
```
示例 2：

输入：strs = ["10", "0", "1"], m = 1, n = 1
输出：2
解释：最大的子集是 {"0", "1"} ，所以答案是 2 。
```
本题中strs 数组⾥的元素就是物品，每个物品都是⼀个.⽽m 和 n相当于是⼀个背包，两个维度的背包,所以本体为01背包问题，
 - dp[i][j]：最多有i个0和j个1的strs的最⼤⼦集的⼤⼩为dp[i][j]
 - dp[i][j] 可以由前⼀个strs⾥的字符串推导出来，strs⾥的字符串有zeroNum个0，oneNum个dp[i][j] 就可以是 dp[i - zeroNum][j - oneNum] + 1。然后我们在遍历的过程中，取dp[i][j]的最⼤值
```
dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1)
```
 - 价值不为负，初始化为0即可
 - 遍历顺序：m和n都为背包容量，那么两者都倒叙即可
 
 java代码如下：
 ```
 class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        int[][] dp = new int[m+1][n+1];
    
        for (String str : strs) {
            int oneNum = 0 , ZeroNum = 0;
            for (char c : str.toCharArray()) {
                if (c == '0') ZeroNum++;
                else oneNum++;
            }
            
            for (int i = m; i >= ZeroNum; i--){
                for (int j = n; j >= oneNum; j--){
                    dp[i][j] = Math.max(dp[i][j], dp[i - ZeroNum][j - oneNum] + 1);
                }
            } 
        }
        
        return dp[m][n];
    }
}
```
