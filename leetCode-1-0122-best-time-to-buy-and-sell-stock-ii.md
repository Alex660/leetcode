# 买卖股票的最佳时机 II（简单）
# 题目描述
![截屏2019-10-31上午7.10.44.png](https://pic.leetcode-cn.com/755e6eda51d45c7a876aff364f68ab63a111e33234ec9fff22df5271278dd4d4-%E6%88%AA%E5%B1%8F2019-10-31%E4%B8%8A%E5%8D%887.10.44.png)
# 题目地址
<https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/solution/mai-mai-gu-piao-de-zui-jia-shi-ji-ii-by-leetcode/>
### 股票6道
+ 1、[121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)
+ 2、[122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)
+ 3、[123. 买卖股票的最佳时机 III](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/submissions/)
+ 4、[309. 最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/submissions/)
+ 5、[188. 买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/submissions/)
+ 6、[714. 买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/submissions/)
## [卍解👇敬请戳看](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/demos/%E8%82%A1%E7%A5%A86%E9%81%93.md)
___
#### 解法一：暴力枚举
+ 时间复杂度：O(n^n) 指数级
+ 空间复杂度：O(n) 递归深度为n
+ 递归枚举所有可能情况，取其中最大值
+ 设可获最大价值max
+ 设当天买卖股票可或临时最大价值maxProfit
+ 设当天start
+ 结果
  + 超出时间限制
```javascript
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
    return calc(prices,prices.length,0)
};
function calc(prices,len,start){
    if(start >= len){
        return 0;
    }
    var max = 0;
    for(var startIndex = start;startIndex<len;startIndex++){
        var maxProfit = 0;
        for(var i = startIndex+1;i<len;i++){
            if(prices[i] > prices[startIndex]){
                // 当前剩余价值+当前价值-第一天起始点价值 == 当前组合的总价值
                var profit = calc(prices,len,i+1) + prices[i] - prices[startIndex];
                // 更新当天与第i天 最大价值和
                if(profit > maxProfit){
                    maxProfit = profit;
                }
            }
        }
        // 更新每天价值最大值的和
        if(maxProfit > max){
            max = maxProfit;
        }
    }
    return max;
}
```
#### 解法二：全局贪婪匹配 == 阶段1贪婪匹配 + 阶段2贪婪匹配 + ... + 阶段n贪婪匹配 如有只要钻石会员不要黄金会员！！！
+ 原理类似正则表达式贪婪匹配模式
  + 趋向于最大长度匹配 
  + 例如
    + str = "abcaxc"
    + patter p = "ab.*c"
    + 匹配结果为 "abcaxc" 
      + 对应本题的在最低点买入，最高点卖出，利润在某个阶段内才会最大
      + 借用官方一张图说明
        ![](https://pic.leetcode-cn.com/d447f96d20d1cfded20a5d08993b3658ed08e295ecc9aea300ad5e3f4466e0fe-file_1555699515174)  
        + 如图所示
          + 假设第一天从valley(i)开始，交易最后一天是在peak(j)结束，求最大利润
            + 遍历
              + 第一个阶段的最低点恰好是起点valley(i)，最高点为peak(i)
                + 当前阶段最大利润是 peak(i) - valley(i)  == A 
              + 第二个阶段的最低的是vallery(j),最高点为peak(j)
                + 当前阶段最大利润是 peak(j) - valley(j)  == B
          + 所以 A+b 即为所求全阶段最大利润
             
  + 应用到本题即是第一个起点开始 **循环**
    1. 若下一个节点是下降 即 第i+1天到价格 小于 第i天的价格
      + 则将起点记录为临时 ***最高价格节点***，i++ ，
        + **循环**
          + 直到第k天小于第k+1天时，说明开始上升
          + 则将第k天记录为从 i到k阶段 的 ***最低价格节点***
      + 则此阶段最大利润等于 最高-最低节点价格 == prices[k] - prices[i]
    2. 若下一个节点是上升 即 第i+1天到价格 大于 第i天的价格
      + 则将起点记录为临时 ***最低价格节点***，i++ ，
        + **循环** 
          + 直到第k天大于第k+1天时，说明开始下降
          + 则将第k天记录为从 i到k阶段 的 ***最高价格节点***
      + 则此阶段最大利润等于 最高-最低节点价格 == prices[k] - prices[i]
   + n轮外循环结束  
     + 所有交易日的最大利润 == 各个阶段的最大利润和 
```javascript
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
    // 谷值
    var valley = prices[0];
    // 峰值
    var peak = prices[0];
    // 最大利润值
    var maxProfit = 0;
    var lenNeed = prices.length - 1;
    var i = 0;
    while(i < lenNeed){
        while(i < lenNeed && prices[i] > prices[i+1]){
            i++;
        }
        valley = prices[i];
        while(i < lenNeed && prices[i] <= prices[i+1]){
            i++;
        }
        peak = prices[i];
        maxProfit += peak - valley;
    }
    return maxProfit;
};
```
#### 解法三：贪心算法 = 相邻两天利润大于0则抢走 一个不留不管下一个 既要芝麻也要西瓜！！！
+ 原理类似正则表达式非贪婪匹配模式
  + 趋向于最短长度匹配 
    + 明天比今天价格高，今天买入明天就卖出
    + 以后的每一天都是如此重复不留到第三天  
  + 例如
    + str = "abcaxc"
    + patter p = "ab.*?c"
    + 匹配结果为 "abc" 
      + 对应本题就是只要第二天比第一天价格上涨就卖出
      + 借用官方一张图说明
      ![](https://pic.leetcode-cn.com/6eaf01901108809ca5dfeaef75c9417d6b287c841065525083d1e2aac0ea1de4-file_1555699697692)  
      + D = A + B +C
        + 而不再需要等遍历到D时再算 peak(D) - valley(A) 
```javascript
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
    // 最大利润值
    var maxProfit = 0;
    var len = prices.length;
    var i = 0;
    for(var i = 1;i<len;i++){
        if(prices[i] > prices[i-1]){
            // 只要明天赚了就抛售获得利润存钱包里
            maxProfit += prices[i] - prices[i-1];
        }
    }
    return maxProfit;
};
```
### 解法四：动态规划
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
            dp[0][0] = 0;
            dp[0][1] = -prices[0];
            continue;
        }
        dp[i][0] = Math.max(dp[i-1][0],dp[i-1][1]+prices[i]);
        dp[i][1] = Math.max(dp[i-1][0]-prices[i],dp[i-1][1]);
    }
    return dp[n-1][0];
};
```
### 解法五：动态规划 + 降维
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
    let dp_i_0 = 0;
    let dp_i_1 = -Infinity;
     for(let i = 0;i < n;i++){
        var tmp = dp_i_0; 
        dp_i_0 = Math.max(dp_i_0,dp_i_1+prices[i]);
        dp_i_1 = Math.max(tmp-prices[i],dp_i_1);
    }
    return dp_i_0;
};
```