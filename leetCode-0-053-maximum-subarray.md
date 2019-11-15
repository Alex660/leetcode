# 最大子序和（简单）
# 题目描述
![截屏2019-11-14下午2.00.53.png](https://pic.leetcode-cn.com/76f1b60d91ffba2cfa8b2961cf48d45ec39f622d559ff3c4528f3fe58f80856f-%E6%88%AA%E5%B1%8F2019-11-14%E4%B8%8B%E5%8D%882.00.53.png)
# 题目地址
<https://leetcode-cn.com/problems/maximum-subarray/>
#### 解法一：暴力枚举
+ 由题意可知，此题是在求连续子组合的最大值
  + 双重循环，内循环初始值跟着外循环后面，确保是在求**连续**子组合的和
  + 当前遍历数字也有可能是最大数即所求组合为一个数；例如[9,-2,-3,-4,-5]
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function(nums) {
    var max = Number.MIN_SAFE_INTEGER;
    for(var i=0;i<nums.length;i++){
        var sum = 0;
        for(var j = i;j<nums.length;j++){
            sum+=nums[j];
            if(sum > max){
                max = sum;
            }
        }
    }
    return max;
};
```
#### 解法二：动态规划
+ 类似题型
  + [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/solution/62-bu-tong-lu-jing-by-alexer-660/) 
  + [63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/solution/63-bu-tong-lu-jing-ii-by-alexer-660/)
  + [120. 三角形最小路径和](https://leetcode-cn.com/problems/triangle/solution/120-san-jiao-xing-zui-xiao-lu-jing-he-by-alexer-66/)
+ 图解
![截屏2019-11-14下午5.35.35.png](https://pic.leetcode-cn.com/fdd2decdfe338e11e894f86dfc8183f4b45aafc627b4c47e480f59ad31030dbb-%E6%88%AA%E5%B1%8F2019-11-14%E4%B8%8B%E5%8D%885.35.35.png)
![截屏2019-11-14下午6.05.26.png](https://pic.leetcode-cn.com/ee7b113a7310d0d2b7d723b3a951d03b201e3bf1b53d1b8710edcbb2dfff7342-%E6%88%AA%E5%B1%8F2019-11-14%E4%B8%8B%E5%8D%886.05.26.png)
+ 分析
  + 求最大子序和意即求**连续**子组合的**最大值**
  + 且nums数组并不具备单调性
  + 如果nums数组中没有负数，则此问题类似[62. 不同路径](https://leetcode-cn.com/problems/unique-paths/solution/62-bu-tong-lu-jing-by-alexer-660/) 
    + **如图B**
      + 设n = nums.length
      + 推出递推方程/状态转移方程为：**dp[i] = Max(dp[i-1]+nums[i],nums[i])**
      + 最佳子结构
        + 当前位置n的最大子序列和由前n-1个数字的最大子序列和推出
      + max = Math.max(max,dp[i]) = dp[n]
      + 且一定是连续的，因为只有数组里所有数字加起来才是最大值
  + 如果nums数组中有负数，则此问题类似[63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/solution/63-bu-tong-lu-jing-ii-by-alexer-660/)
    + **如图A**
      + 即将数组里的负数当中机器人行走遇到的障碍物
      + 因为题目要求最大子序和
        + 63题中，机器人向前走，虽然遇到障碍物，但是跳过后前面经过的路径依旧有效，算在里面；
        + 试想，如果一个数加上一个负数那么和一定小于前一个数
          + 因此宁可不加，即将负数当作障碍物，跳过不计入子序和里面；
            + 因此跳过不加当前负数的话，可以得出当前组合的最大和
            + 但不是全组合中最大和
          + 如果跳过，那么前面的和就必须无效了；因为题意要求子序列必须连续
            + 但是跳过负数求和的子序列不一定比下一个加了负数的组合大
            + 如图dp[6] = dp[5] + dp[4] > dp[3]、其中dp[4] 加了负数-1
          + 例如图中
            + dp[0]:[-2]
            + dp[1]:[-2,1]
            + dp[2]:[-2,1,-3]
            + 分析
              + 首字母当作最大值用于比较后面的数无可厚非
              + dp[1]，因前面一个组合dp[0]是负数，跳过不加，表现在程序中就是+0，最大值为当前值
              + dp[2],因前面一个组合dp[1]是正数，因此加上当前值一定比当前值大，所以最大值为前一个组合的和+当前值
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function(nums) {
    if(!nums || nums.length == 0){
        return 0;
    }
    var max = nums[0];
    var n = nums.length;
    var dp = new Array(n);
    dp[0] = max;
    for(var i=0;i<n;i++){
        dp[i] = (dp[i-1] > 0 ? dp[i-1] : 0) + nums[i];
        max = Math.max(max,dp[i]);
    }
    return max;
};
```
+ 或者这样写你更好理解
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function(nums) {
    var max = nums[0];
    var dp = new Array(nums.length);
    dp[0] = max;
    for(var i = 1;i < nums.length;i++){
        dp[i] = Math.max(dp[i-1] + nums[i],nums[i]);
        max = Math.max(max,dp[i]);
    }
    return max;
};
```
+ 优化版
  + dp[n-1]实质是在求前n-1个数字的和
    + 正数则加上当前nums[n]值构成最大和即最大子序列
    + 负数，则跳过，以当前值为最大子序列
      + 因为一个数加上一个负数一定会变小，哪怕当前数是负数也比加了的和要大
  + 此解法也是以上解法中运行最快的。
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function(nums) {
    if(!nums || nums.length == 0){
        return 0;
    }
    var max = nums[0];
    var n = nums.length;
    var sum = 0;
    for(var i=0;i<n;i++){
        if(sum > 0){
            sum += nums[i];
        }else{
            sum  = nums[i]
        }
        max = Math.max(max,sum);
    }
    return max;
};
```
+ 优化++
  + 当然要是不喜欢循环内判断if,else
  + 还可以优化
    + 循环内的判断if/else代码块是在求sum和sum+nums[i]的最大值
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function(nums) {
    var max = Number.MIN_SAFE_INTEGER;
    var prev = 0;
    for(var i=0;i<nums.length;i++){
        prev = Math.max(prev+nums[i],nums[i])
        max = Math.max(max,prev);
    }
    return max;
};
```