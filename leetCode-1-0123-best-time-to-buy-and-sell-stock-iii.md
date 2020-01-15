# 买卖股票的最佳时机 III（困难）
# 题目描述
![截屏2019-11-18上午10.54.48.png](https://pic.leetcode-cn.com/34551a9f4099b9da3b44c6355551d4cfa48a542c65bb6789bf8b5ceabf596fb2-%E6%88%AA%E5%B1%8F2019-11-18%E4%B8%8A%E5%8D%8810.54.48.png)
# 题目地址
<https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/>
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
    let maxTime = 2;
    let dp = Array.from(new Array(n),() => new Array(maxTime+1));
    for(let i = 0;i < n;i++){
        for(let r = 0;r <= maxTime;r++){
            dp[i][r] = new Array(2).fill(0);
        }
    }
    for(let i = 0;i < n;i++){
        for(let k = maxTime;k >= 1;k--){
            if(i == 0){
                dp[i][k][0] = 0;
                dp[i][k][1] = -prices[i];
                continue;
            }
            dp[i][k][0] = Math.max(dp[i-1][k][0],dp[i-1][k][1] + prices[i]);
            dp[i][k][1] = Math.max(dp[i-1][k][1],dp[i-1][k-1][0] - prices[i]);
        }
    }
    return dp[n-1][maxTime][0];
};
```
#### 解法二：动态规划-降维
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
    let dp_i_1_0 = 0;
    let dp_i_1_1 = -Infinity;
    let dp_i_2_0 = 0;
    let dp_i_2_1 = -Infinity;
    for(let i = 0;i < n;i++){
        dp_i_1_0 = Math.max(dp_i_1_0,dp_i_1_1 + prices[i]);
        dp_i_1_1 = Math.max(dp_i_1_1,0 - prices[i]);
        dp_i_2_0 = Math.max(dp_i_2_0,dp_i_2_1+prices[i]);
        dp_i_2_1 = Math.max(dp_i_2_1,dp_i_1_0-prices[i]);
    }
    return dp_i_2_0;
};
```