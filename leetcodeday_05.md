# 1049. 最后一块石头的重量 II
## 题目描述
有一堆石头，用整数数组 stones 表示。其中 stones[i] 表示第 i 块石头的重量。
每一回合，从中选出任意两块石头，然后将它们一起粉碎。假设石头的重量分别为 x 和 y，且 x <= y。那么粉碎的可能结果如下：
如果 x == y，那么两块石头都会被完全粉碎；
如果 x != y，那么重量为 x 的石头将会完全粉碎，而重量为 y 的石头新重量为 y-x。
最后，最多只会剩下一块 石头。返回此石头 最小的可能重量 。如果没有石头剩下，就返回 0。
## 示例
```
输入：stones = [2,7,4,1,8,1]
输出：1
解释：
组合 2 和 4，得到 2，所以数组转化为 [2,7,1,8,1]，
组合 7 和 8，得到 1，所以数组转化为 [2,1,1,1]，
组合 2 和 1，得到 1，所以数组转化为 [1,1,1]，
组合 1 和 1，得到 0，所以数组转化为 [1]，这就是最优值。
```
```
输入：stones = [31,26,33,21,40]
输出：5
```
```
提示
1 <= stones.length <= 30
1 <= stones[i] <= 100
```
本题就是尽量(石头weight可能为奇数)让⽯头分成重量相同的两堆，相撞之后剩下的⽯头最⼩，这样就化解成01背包问题了
物品的重量为store[i]，物品的价值也为store[i]。对应着01背包⾥的物品重量weight[i]和 物品价值value[i]
 - dp[j]表⽰容量为j的背包，最多可以背dp[j]这么重的⽯头
 - 递推公式：dp[j] = max(dp[j], dp[j - stones[i]] + stones[i])
 - 此题提示最大为30*100，那么容量我们可以算出来为1501就行，如何初始化dp[j]呢，因为重量都不会是负数，所以dp[j]都初始化为0就可以了，即Arrays.fill(dp,0)
 - 遍历和分割那题一样
dp[target]⾥是容量为target的背包所能背的最⼤重量。
那么分成两堆⽯头，⼀堆⽯头的总重量是dp[target]，另⼀堆就是weight - dp[target]。
在计算target的时候，target = weight / 2 因为是向下取整，所以sum - dp[target] ⼀定是⼤于等于dp[target]的。
那么相撞之后剩下的最⼩⽯头重量就是 (sum - dp[target]) - dp[target]。

java代码
```
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int[] dp = new int[1501];
        Arrays.fill(dp,0);
        int weight = 0;
        for(int i=0; i < stones.length;i++){
            weight += stones[i];
        }
        int target = weight/2;
        for(int i = 0;i <stones.length;i++){
            for(int j = target;j >= stones[i];j--){
                dp[j] = Math.max(dp[j],dp[j-stones[i]]+stones[i]);
            }
        }
        return weight - dp[target] - dp[target];
    }
}
```
