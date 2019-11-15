# 打家劫舍 II（中等）
# 题目描述
![截屏2019-11-15下午7.21.08.png](https://pic.leetcode-cn.com/20fef5818b951b5fd4deb0641303db111d6d85d0b79bea23a3481cf25917e396-%E6%88%AA%E5%B1%8F2019-11-15%E4%B8%8B%E5%8D%887.21.08.png)
# 题目地址
<https://leetcode-cn.com/problems/house-robber-ii/>
#### 类似题型
+ [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/solution/198-da-jia-jie-she-by-alexer-660/)
  + 所有解法跟198题解法几乎完全一样
  + 区别就是本题第一个房子和最后一个房子连在一起，而且同样保留198题的相邻房子不能同时偷的原则
  + 所以此题可以分为两种情况
    + 偷第一家，不能偷最后一家
    + 不偷第一家，能偷最后一家
  + 因此在代码中，直接截取掉第一个和最后一个数字分解成两个子问题求解即可。
#### 解法一：动态规划
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    var n = nums.length;
    if(n == 1){
        return nums[0];
    }else if(n == 0){
        return 0;
    }
    function dpGO(nums){
       var n = nums.length;
       var dp = Array.from(new Array(n),() => new Array(n));
       dp[0][0] = 0;
       dp[0][1] = nums[0];
       for(var i = 1;i < nums.length;i++){
           dp[i][0] = Math.max(dp[i-1][0],dp[i-1][1]);
           dp[i][1] = nums[i]+dp[i-1][0];
       }
       return Math.max(dp[n-1][0],dp[n-1][1]);
    }
    var need1 = dpGO(nums.slice(1));
    var need2 = dpGO(nums.slice(0,nums.length-1));
    return Math.max(need1,need2);
};
```
#### 解法二：动态规划降维
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    var n = nums.length;
    if(n == 1){
        return nums[0];
    }else if(n == 0){
        return 0;
    }
    function dpGO(nums){
        var dp = new Array(n-1);
        dp[0] = 0;
        dp[1] = nums[0];
        for(var i = 2;i < n;i++){
            dp[i] = Math.max(dp[i-1],dp[i-2]+nums[i-1]);
        }
        return dp[n-1];
    }
    var need1 = dpGO(nums.slice(1));
    var need2 = dpGO(nums.slice(0,nums.length-1));
    return Math.max(need1,need2);
};
```
#### 解法三：动态规划-去维
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    var n = nums.length;
    if(n == 1){
        return nums[0];
    }else if(n == 0){
        return 0;
    }
    function dpGO(nums){
       var prevMax = 0;
       var currMax = 0;
       for(var i = 0;i < nums.length;i++){
           var tmp = currMax;
           currMax = Math.max(currMax,prevMax+nums[i]);
           prevMax = tmp;
       }
       return currMax;
    }
    var need1 = dpGO(nums.slice(1));
    var need2 = dpGO(nums.slice(0,nums.length-1));
    return Math.max(need1,need2);
};
```
#### 解法四：间隔步数
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    var n = nums.length;
    if(n == 1){
        return nums[0];
    }else if(n == 0){
        return 0;
    }
    function dpGO(nums){
        var oddSum = 0;
        var evenSum = 0;
        for(var i = 0;i<nums.length;i++){
            if(i%2  == 0){
                evenSum = Math.max(evenSum+nums[i],oddSum);
            }else{
                oddSum = Math.max(oddSum+nums[i],evenSum);
            }
        }
        return Math.max(oddSum,evenSum);
    }
    var need1 = dpGO(nums.slice(1));
    var need2 = dpGO(nums.slice(0,nums.length-1));
    return Math.max(need1,need2);
};
```