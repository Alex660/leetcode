# 买卖股票的最佳时机（简单）
# 题目描述
![截屏2019-11-18上午10.06.19.png](https://pic.leetcode-cn.com/30370fda8f7e0d197197b030b21913504d3aaaa374241c9ef3c6a867ffc43d96-%E6%88%AA%E5%B1%8F2019-11-18%E4%B8%8A%E5%8D%8810.06.19.png)
# 题目地址
<https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/>
### 股票6道
+ 1、[121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)
+ 2、[122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)
+ 3、[123. 买卖股票的最佳时机 III](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/submissions/)
+ 4、[309. 最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/submissions/)
+ 5、[188. 买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/submissions/)
+ 6、[714. 买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/submissions/)
## [卍解👇敬请戳看](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/demos/%E8%82%A1%E7%A5%A86%E9%81%93.md)
___
#### 解法一：动态规划
```javascript
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
    let n = prices.length;
    if(n == 0){
        return 0;
    }
    let dp = Array.from(new Array(n),() => new Array(2));
    for(let i = 0;i < n;i++){
        if(i == 0){
            dp[i][0] = 0;
            dp[i][1] = -prices[i];
            continue;
        }
        dp[i][0] = Math.max(dp[i-1][0],dp[i-1][1]+prices[i]);
        dp[i][1] = Math.max(-prices[i],dp[i-1][1]);
    }
    return dp[n-1][0];
};
```
#### 解法二：动态规划 + 降维
```javascript
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
    let n = prices.length;
    if(n == 0){
        return 0;
    }
    var dp_i_0 = 0;
    var dp_i_1 = -Infinity;
    for(let i = 0;i < n;i++){
        dp_i_0 = Math.max(dp_i_0,dp_i_1 + prices[i]);
        dp_i_1 = Math.max(-prices[i],dp_i_1);
    }
    return dp_i_0;
};
```
#### 解法三：暴力法
+ 空间复杂度：O(n^2)
+ 时间复杂度：O(1)
+ 因限制交易一笔
+ 所以可以枚举出所有的交易组合维护max = max(prices[j] = prices[i])即可
```javascript
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
    let max = 0;
    for(var i = 0;i <prices.length - 1;i++){
        for(var j = i+1;j < prices.length;j++){
            max = Math.max(max,prices[j] - prices[i])
        }
    }
    return max;
};
```
#### 解法四：峰谷法
+ 时间复杂度：O(n)
+ 空间复杂度：O(1)
+ [参考122. 买卖股票的最佳时机 II-解法二](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/solution/122-mai-mai-gu-piao-de-zui-jia-shi-ji-ii-by-alexer/)
```javascript
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
    let valley = Number.MAX_SAFE_INTEGER;
    let max = 0;
    for(var i = 0;i <prices.length;i++){
        if(prices[i] < valley){
            valley = prices[i];
        }else{
            max = Math.max(max,prices[i] - valley);
        }
    }
    return max;
};
```