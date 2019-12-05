# 使用最小花费爬楼梯（简单）
# 题目描述
![截屏2019-12-02下午11.19.34.png](https://pic.leetcode-cn.com/3de17576540c5f05e03a28c19a1399093235a735c68fd5c932a17c6803a40335-%E6%88%AA%E5%B1%8F2019-12-02%E4%B8%8B%E5%8D%8811.19.34.png)
# 题目地址
<https://leetcode-cn.com/problems/min-cost-climbing-stairs/solution/>
## 解法四：
![截屏2019-12-03上午6.47.40.png](https://pic.leetcode-cn.com/a2de902a8f4ff46c3895f8af970db93e824ae92af58dc5a4b005d01280ea23a2-%E6%88%AA%E5%B1%8F2019-12-03%E4%B8%8A%E5%8D%886.47.40.png)
#### 解法一：动态规划 - 分解
+ 思路
  + 如果走一步或两步上来有两种方法
    + [70. 爬楼梯-解法三](https://leetcode-cn.com/problems/climbing-stairs/solution/70-pa-lou-ti-by-alexer-660/)
    + 与70题一模一样
    + 动态转移方程为
      + **dp[i] = Math.min(dp[i-2] , dp[i-1]) + cost[i]**
  + 由题意分解可知
    + 如果最后一步正好是在最后一个台阶上时，最后一个台阶的花费值不算在内
    + 动态转移方程为
      + **i == n && dp[i] = Math.min(dp[i-2] , dp[i-1])**
```javascript
/**
 * @param {number[]} cost
 * @return {number}
 */
var minCostClimbingStairs = function(cost) {
    let n = cost.length;
    let dp = new Array(n+1).fill(0);
    dp[0] = cost[0]; 
    dp[1] = cost[1]; 
    for(let i = 2;i <= n;i++){
        if(i == n){
            dp[i] = Math.min(dp[i-2] , dp[i-1]);
        }else{
            dp[i] = Math.min(dp[i-2] , dp[i-1]) + cost[i];
        }
    }
    return dp[n];
};
```
#### 解法二：动态规划 - 合并
+ 由解法一可知
  + 既然有最后一阶台阶不算的情况在内，就直接在最后加一个0(表示包含在这种特殊情况在内，加0就等于花费值为0嘛)
  + 这样动态转移方程就不用变了
    + **dp[i] = Math.min(dp[i-2] , dp[i-1]) + cost[i]**
```javascript
/**
 * @param {number[]} cost
 * @return {number}
 */
var minCostClimbingStairs = function(cost) {
    cost.push(0);
    let n = cost.length;
    let dp = [];
    dp[0] = cost[0]; 
    dp[1] = cost[1]; 
    for(let i = 2;i < n;i++){
        dp[i] = Math.min(dp[i-2] , dp[i-1]) + cost[i];
    }
    return dp[n-1];
};
```
#### 解法三：动态规划 - 比较
+ 由以上解法可知
  + 更通俗的解法是
    + 唯一不同的情况是最后一阶的台阶的花费值是否被算在内
    + 那么我统一加最后一阶台阶，只在最后取结果的时候
      + 比较判断去掉最后一个台阶的走法情况是否花费更少即可
```javascript
/**
 * @param {number[]} cost
 * @return {number}
 */
var minCostClimbingStairs = function(cost) {
    let n = cost.length;
    let dp = [];
    dp[0] = cost[0]; 
    dp[1] = cost[1]; 
    for(let i = 2;i < n;i++){
        dp[i] = Math.min(dp[i-2] , dp[i-1]) + cost[i];
    }
    return dp[n-1] > dp[n-2] ? dp[n-2] : dp[n-1];
};
```
#### 解法四：去维 - 变量
+ 通过以上分析
  + 用两个变量，结合解法三的比较，可得当前所有解法中时间复杂度最优的解法如下
```javascript
/**
 * @param {number[]} cost
 * @return {number}
 */
var minCostClimbingStairs = function(cost) {
    let n = cost.length;
    let pre = cost[0]; 
    let next = cost[1]; 
    for(let i = 2;i < n;i++){
        let tmp = next;
        next = Math.min(pre,next)+cost[i];
        pre = tmp;
    }
    return Math.min(pre,next);
};
```