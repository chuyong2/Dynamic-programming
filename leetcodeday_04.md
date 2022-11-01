# leetcode416 分割等和子集
## 题目描述
给你一个只包含正整数的非空数组nums请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。
```
示例 1：
输入：nums = [1,5,11,5]
输出：true
解释：数组可以分割成 [1, 5, 5] 和 [11] 
```
```
示例 2：
输入：nums = [1,2,3,5]
输出：false
解释：数组不能分割成两个元素和相等的子集
1 <= nums.length <= 200
1 <= nums[i] <= 100
```
### 基本转换思路
题目描述是正整数集合，我们是否可以找到一些数的和为整个数组和的一半，因此这个问题可以转换成「0−1 背包问题」。
这道题与传统的「0−1 背包问题」的区别在于，传统的「0−1 背包问题」要求选取的物品的重量之和不能超过背包的总容量。
这道题则要求选取的数字的和恰好等于整个数组的元素和的一半。

###基础条件
 - 首先我们确定数组长度，即背包大小，我们从题目中可以看出最多200个数且最大为100，因此我们背包容量取200*100/2+1就行，即10001；
 - 再者我们求一下nums数组的和，如果数组和为奇数，直接返回false，为偶数可以继续做下一步
### 动态规划步骤
 - dp[j]表示背包容量为j，里面的数可以凑得子集总和为dp[j]
 - 递推公式：dp[j] = max(dp[j],dp[j-nums[i]+nums[i])根据01背包推得
 - 初始化：首先背包容量为0，那么dp[0] = 0;此题数都大于0，所以非0下表都为0就可以（这样才能让dp数组在递归公式的过程中取的最⼤的价值，⽽不是被初始值覆盖了。）
 - 遍历顺序：使用一维数组的话，外层是物品，内层是背包，即外层是nums数组，内层为背包，且内层循环倒叙遍历
java代码(完整代码，可以自己debug)
```
public class leetcode416 {
    public static boolean canPartition(int[] nums) {
        int[] dp = new int[10001];
        Arrays.fill(dp,0);
        int sum=0;
        for(int i = 0;i< nums.length;i++)
        {
            sum += nums[i];
        }
        if(sum % 2 != 0){
            return false;
        }
        int target = sum/2;
        dp[0] = 0;
        for(int i = 0;i < nums.length;i++){
            for(int j = target;j >= nums[i];j--){
                dp[j] = Math.max(dp[j],dp[j-nums[i]]+nums[i]);
            }
        }
        if(dp[target] == target)  return true;
        return false;
    }

    public static void main(String[] args) {
        System.out.println(canPartition(new int[]{1,4,3,10,2}));
    }
}
```



