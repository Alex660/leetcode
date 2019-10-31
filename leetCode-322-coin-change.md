# 零钱兑换（中等）
# 题目描述
![截屏2019-10-30上午9.41.41.png](https://pic.leetcode-cn.com/69405d6ef385f7d92930f1a82eb2abfa81db68a4715f120de14d4e61e60112f3-%E6%88%AA%E5%B1%8F2019-10-30%E4%B8%8A%E5%8D%889.41.41.png)
# 题目地址
<https://leetcode-cn.com/problems/coin-change/solution/>
#### 解法一：暴力递归枚举
+ 时间超出限制
+ 通过每次将需要求的总价值除以币值组的每一个值，作为当前币种币值可放入的最大个数
+ 然后递归求剩下需要凑足的价值能够装满当前币值的个数
  + 依次从0到当前最大值个数遍历，每次加x * coins[i]的价值，然后再减去此价值求剩余的价值重复第一步
+ 针对所有可以拼凑出解雇的组合取最小值 可以通过每次更新维护一个最小值实现
+ 原理
  + 通过每次遍历求 总价值除以当前币值的商为当前币值可以装入的最大值
    + 从0 遍历到 此最大值，组合n个当前币值+递归求剩余价值需要剩余币值各自所需个数  
    + 设 S 为所需求最大金额，c(i) 为每个币值 依次求 S/c(i) 并从0遍历到此商，递归组合其它币值
    + （ S/c(1) * S/(c(2) * S/(c(3)) * ... ) = S^n / (c(1)*c(2)*c(3)*...*c(n))
+ 时间复杂度：O(S^n) 指数级
+ 空间复杂度：O(n) 最坏情况下 每个币值都用到 
```javascript
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */
var coinChange = function(coins, amount) {
    var len = coins.length;
    var tmpNum = 0;
    var tmpOneAmount = 0;
    function coinDispenser(i,tmpAmount){
        if(tmpAmount == 0){
            return 0;
        }
        if(i < len && tmpAmount > 0){
            var minNum = Number.POSITIVE_INFINITY;  
            var maxValue = parseInt(tmpAmount/coins[i]);
            for(let x = 0;x <= maxValue;x++){
                tmpOneAmount = x * coins[i]; 
                if(tmpOneAmount <= tmpAmount){
                    tmpNum = coinDispenser(i+1,tmpAmount - tmpOneAmount);
                    if(tmpNum != -1){
                        minNum = Math.min(minNum,tmpNum + x);
                    }
                }
            }
            return minNum == Number.POSITIVE_INFINITY  ? -1 : minNum;
        }
        return -1;
    }
    return coinDispenser(0,amount);
};
```
#### 解法二：暴力递归枚举+备忘录
+ 备忘录
  + 通过每轮i和amount组合为数组键值确保唯一性 
+ 但还是会超出时间限制
  + 原因大部分是不仅没有脱离指数级限制还有加乘和整除操作而这两样计算机计算性能较差
```javascript
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */
var coinChange = function(coins, amount) {
    var len = coins.length;
    var tmpNum = 0;
    var tmpOneAmount = 0;
    var arr = new Array();
    function coinDispenser(i,tmpAmount){
        if(tmpAmount == 0){
            return 0;
        }
        var tmpArr = arr[i+String(tmpAmount)];
        if(tmpArr != undefined){
            return tmpArr;
        }
        if(i < len && tmpAmount > 0){
            var minNum = Number.POSITIVE_INFINITY;  
            var maxValue = parseInt(tmpAmount/coins[i]);
            for(let x = 0;x <= maxValue;x++){
                tmpOneAmount = x * coins[i]; 
                if(tmpOneAmount <= tmpAmount){
                    tmpNum = coinDispenser(i+1,tmpAmount - tmpOneAmount);
                    if(tmpNum != -1){
                        minNum = Math.min(minNum,tmpNum + x);
                    }
                }
            }
            return tmpArr = Number.isFinite(minNum)  ? minNum : -1;
        }
        return -1;
    }
    return coinDispenser(0,amount);
};
```
#### 解法三：递归+子结构
+ 上面两个解法中主要通过求整，加乘(x*coins[i])，最坏情况下每个币值都用经过n轮遍历，时间复杂度为n次方即指数级
+ 换一种思路 最优子结构
  + 递推公式
    + n 为目标余额，c[i]为硬币面值
    + S(n) = 0 , n == 0 时
    + S(n) = 1 + min(S(n-c[i])) , i>=1 && i<=k
+ 但依旧是超时 
```javascript
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */
var coinChange = function(coins, amount) {
    if (amount === 0) return 0;
    let min = Infinity;
    for (let coin of coins) {
        if (coin <= amount){
            let tmp = coinChange(coins, amount - coin);
            if (tmp !== -1){
                min = Math.min(min, tmp+1);
            }
        }
    }
    return Number.isFinite(min) ? min : -1;
};
```
#### 解法四：递归+子结构+备忘录
+ 备忘录
  + 通过amount作为键值
    + 所以备忘录模式下之前组合过的amout 不再重复计算 
+ ***效果喜人***
  + ***不再超出时间限制***  
  + ***性能very good !***
```javascript
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */
var coinChange = function(coins, amount) {
    var arr = new Array(amount+1).fill(-2);
    return coinHelp(coins,amount,arr);
};
function coinHelp(coins,amount,arr){
    if (amount <= 0) return 0;
    if(arr[amount] != -2)return arr[amount];
    let min = Infinity;
    for (let coin of coins) {
        if (amount >=  coin){
            let tmp = coinHelp(coins, amount - coin,arr);
            if (tmp != -1){
                min = Math.min(min, tmp+1);
            }
        }
    }
    return arr[amount] = Number.isFinite(min) ? min : -1;
}
```
#### 解法五：动态规划 = 递归+最优子结构+备忘录+自上至下
+ 自上至下
  + 目标值amount从amount到0 
+ 由解法三、四可知状态转移方程
+ 且上面两种解法存在重复计算子问题 虽然解法四加了备忘录，但是
  ```javascript
  if (tmp != -1){  
    min = Math.min(min, tmp+1);  
  }
  ```
+ 上面这步操作依旧增加了其它子结构解进来比较的机会 所以min值不是原来一步一步加1上来的值会变
+ 动态规划
  + 要改成***最优***子结构 即 原问题的解由子问题的***最优***解构成
  ```javascript
  if (res >= 0 && res < min){
    min = 1 + res;
  }
  ```
  + 此处保证了min值会一直是最优解  大于min的子解不会加进来 小于min的一定是最优解
```javascript
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */
var coinChange = function(coins, amount) {
    var arr = new Array(amount+1).fill(-2);
    return coinHelp(coins,amount,arr);
};
function coinHelp(coins,amount,arr){
    if (amount <= 0) return 0;
    if(arr[amount] != -2)return arr[amount];
    let min = Infinity;
    for (let coin of coins) {
        if (amount >=  coin){
            let tmp = coinHelp(coins, amount - coin,arr);
            if (tmp >= 0 && tmp < min){
                min = Math.min(min, tmp+1);
            }
        }
    }
    return arr[amount] = Number.isFinite(min) ? min : -1;
}
```
#### 解法六：动态规划 = 最优子结构+备忘录+自下至上
+ 事实上解法五 因为递归 在测试值比较大比如上万的时候 会引起堆栈溢出 亦不是最优的动态规划
+ 改成 非递归即从子问题推出大问题的解
  + 自下至上
    + 目标值amount从0到amount
    + 此时函数调用栈的情况是调用一个立刻返回 不会一直压函数入调用栈 再统一出栈求解 解法五就是如此
+ 其它同解法五
+ 此解法最优 测试数据很大也不会超出时间限制
+ 代码中
  > fill(amount+1) 和 dp[amount] > amount
  + 意思是最后比较动态规划的最后一步是否与初始值有变化，有就说明有解，没有则返回-1无解
  + 等价于
    +  fill(amount+2) 和 dp[amount] == amount+2 ？ -1 ： dp[amount]
```javascript
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */
var coinChange = function(coins, amount) {
    if(amount < 1) return 0;
    var dp = new Array(amount+1).fill(amount+1);
    dp[0] = 0;
    for(var i = 0;i<=amount;i++){
        for(var r = 0;r<coins.length;r++){
            if(coins[r] <= i){
                dp[i] = Math.min(dp[i],dp[i-coins[r]] + 1);
            }
        }
    }
    return dp[amount] > amount ? -1 : dp[amount];
};
```