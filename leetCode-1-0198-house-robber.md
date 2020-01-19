# 打家劫舍（简单）
# 题目描述
![截屏2019-11-15下午1.46.23.png](https://pic.leetcode-cn.com/5c1e4fa55d29c41e72d682548070ac39d25ecc858b357dca87b5219f9bacdff5-%E6%88%AA%E5%B1%8F2019-11-15%E4%B8%8B%E5%8D%881.46.23.png)
# 题目地址
<https://leetcode-cn.com/problems/house-robber/>
#### 解法一：动态规划
+ 思路
  + 相邻两房不能同时光顾，求能够偷的的最高金额，金额恒为正数
  + 假设没有相邻禁偷限制，若想前n个房屋内偷到最高
    + dp[n] = dp[n-1] + nums[n]
    + 由此可想到斐波那契数列，从而想到动态规划解决
  + DP动态规划
    + 子问题(重复性)
      + dp[i]：0~i 能偷到到的 max value
      + dp[i] = dp[i-1] +nums[i]
    + 状态定义
      + 第i个偷
        + dp[i][1]，增加维度，二维1代表偷当前i房
      + 第i个不偷
        + dp[i][0]，增加维度，二维0代表不偷当前i房
    + DP方程
      + 第i个不偷
        + dp[i][0] = Math.max(dp[i-1][0],dp[i-1][1])
        + 不知道i-1是否能偷，偷也不违背相邻原则
      + 第i个偷
        + dp[i][1] = nums[i] + Math.max(dp[i-1][0]+0) = nums[i] + dp[i-1][0]
        + 第i房偷，则先加nums[i]之金额，且第i-1就一定不能偷，否则违背相邻原则
        + 至于dp[i-2]则不需重复考虑，因其已在dp[i-1]的子问题当中了
    + 总结状态转移方程为
      + **dp[i][0] = Math.max(dp[i-1][0],dp[i-1][1]);**
      + **dp[i][1] = dp[i-1][0] + nums[i];**
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    var n = nums.length;
    if(n == 0){
        return 0;
    }else if(n == 1){
        return nums[0];
    }
    var dp = Array.from(new Array(n),() => new Array(n).fill(0));
    dp[0][0] = 0;
    dp[0][1] = nums[0];
    for(var i = 1;i < n;i++){
        dp[i][0] = Math.max(dp[i-1][0],dp[i-1][1]);
        dp[i][1] = dp[i-1][0] + nums[i];
    }
    return Math.max(dp[n-1][0],dp[n-1][1]);
};
```
#### 解法二：动态规划-降维
+ 解法一中，我们用二层维度**0**和**1**代表第**i**个房子是否要偷；
  + 因而判断出**dp[n] = dp[n-1] + nums[n]**递推公式无法确定dp[n-1]是否要偷
  + 那么我们可以将偷与不偷的维度压缩在一层维度内，即对dp[n-1]的取值做判断
+ 我们用dp[i]表示：**0~i的能偷的最大值，且第i个必偷或者不必偷**；即第i个偷或者不偷情况下前i个能偷的最大值  
  + 那么dp[i]：Max(dp[i-1]+0,dp[i-2]+nums[i])
  + dp[i-2]+nums[i]：表示第i个必偷下的结果
  + dp[i-1]+0：表示第i个不偷情况下前i-1个偷的最大结果
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    var n = nums.length;
    if(n == 0){
        return 0;
    }else if(n == 1){
        return nums[0];
    }
    var dp = new Array(n);
    dp[0] = nums[0];
    dp[1] = Math.max(nums[0],nums[1]);
    var result = Math.max(dp[0],dp[1]);
    for(var i = 2;i < n;i++){
        dp[i] = Math.max(dp[i-1],dp[i-2]+nums[i]);
        result = Math.max(result,dp[i]);
    }
    return result;
};
```
+ 优化版+1
  + 更符合dp情况的代码描述
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    var n = nums.length;
    if(n == 0){
        return 0;
    }else if(n == 1){
        return nums[0];
    }
    var dp = new Array(n);
    // dp[i] :Max(dp[i-1]+0,dp[i-2]+nums[i])
    dp[0] = nums[0];
    dp[1] = Math.max(nums[0],nums[1]);
    for(var i = 2;i < n;i++){
        dp[i] = Math.max(dp[i-1],dp[i-2]+nums[i]);
    }
    return dp[n-1];
};
```
+ 优化版+2
  + 优化点
    ```javascript
        else if(n == 1){
            return nums[0];
        }
        var dp = new Array(n);
        // dp[i] :Max(dp[i-1]+0,dp[i-2]+nums[i])
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0],nums[1]);
    ```
    + 用dp[i]表示：0~i的能偷的最大值，**且第i-1个必偷**；即第i-1个必偷情况下前i-1个能偷的最大值
      + 这样能将n=1的情况下放到遍历中去
      + 且初始最大值为第一个不需要再跟第二个再做一层比较
  + 结果
    + 性能比上述高
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    var n = nums.length;
    if(n == 0){
        return 0;
    }
    var dp = new Array(n);
    dp[0] = 0;
    dp[1] = nums[0];
    for(var i = 2;i <= n;i++){
        dp[i] = Math.max(dp[i-1],dp[i-2]+nums[i-1]);
    }
    return dp[n];
};
```
#### 解法三：动态规划-去维
+ 空间复杂度：O(1)
+ 由以上解法可知
  + dp[i]的值，只与dp[i-1]和dp[i-2]的值有关，对应不偷当前i个房子和偷当前第i个房子
  + 所以只需要维护两个变量即可
    + 不到更新两个变量的值来保存前n-1个和前n-2个的值即可
    + 到了第n个时，n-1就变为n个的值,n-2就变为n-1个的值，依次递推，最后一个即为所求
    + 在此我们用
      + preMax：n-2
      + currMax：n-1
      + 因此当前i：n
        + 并且更新currMax为当前n
        + preMax更新为上一个currMax
      + 重复即可
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    var n = nums.length;
    if(n == 0){
        return 0;
    }
    var prevMax = 0;
    var currMax = 0;
    for(var i = 0;i < n;i++){
        var tmp = currMax;
        currMax = Math.max(currMax,prevMax+nums[i]);
        prevMax = tmp;
    }
    return currMax;
};
```
#### 解法四：间隔步数
+ 如题相邻两间房间任何情况下都不可同时踏入
  + 假设增加限制要求一步只能踏入1个或2个房间的距离时
    + 那么只有两种方法去偷
      + 要么一次踏2步去下手，即1、3、5、7、9、2n-1
      + 要么一次踏1步去下手，即0、2、4、6、8、10、2n
    + 因此踏两步找到第奇数个房子时  
      + oddSum = oddSum + nums[i]
      + 但是房子可偷价值不一，踏奇数位的走法不一定比踏偶数位的走法更好
        + 所以每次要比较两种走法的最大值
      + 如某个阶段因此取了相反走法的一方值作为最大
        + 则说明当前房子为止最大价值的走法是交替奇偶位走法
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
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
};
```