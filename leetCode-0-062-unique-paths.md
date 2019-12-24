# 不同路径（中等）
# 题目描述
![截屏2019-11-12上午5.55.05.png](https://pic.leetcode-cn.com/28dff34159d26fb870eb284bf45b77ccd46dbaaf068718923dcb41ff88ec6006-%E6%88%AA%E5%B1%8F2019-11-12%E4%B8%8A%E5%8D%885.55.05.png)
![截屏2019-11-12上午5.55.14.png](https://pic.leetcode-cn.com/e34d5dc011055032deb91b38f127f1eff0497f0c294db007478a66a729787fec-%E6%88%AA%E5%B1%8F2019-11-12%E4%B8%8A%E5%8D%885.55.14.png)
# 题目地址
<https://leetcode-cn.com/problems/unique-paths/submissions/>
#### 解法一：动态规划
+ 时间复杂度：O(m*n)
+ 机器人只能向右或向下移动一步
  + 那么从左上角到右下角的走法 = 从右边开始走的路径总数+从下边开始走的路径总数 
  + 所以可推出动态方程为
    + dp[i][j] = dp[i-1][j]+dp[i][j-1]
    + 初始化第一行和第一列的值
      + dp[0][j] = 1，dp[i][0] = 1
      + 因为一直向下或者一直向右走而不转向的话只有一种走法
+ 图解（7*3）
  ![截屏2019-11-12上午11.03.32.png](https://pic.leetcode-cn.com/38504ebd2c5e6d065b7b275d32eb28d9aaba3a5742507765e44e698adddce593-%E6%88%AA%E5%B1%8F2019-11-12%E4%B8%8A%E5%8D%8811.03.32.png) 
```javascript
/**
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
var uniquePaths = function(m, n) {
    var dp = new Array(m);
    for(var i = 0;i<n;i++){
        dp[i] = new Array(m);
        dp[i][0] = 1;
    }
    for(var r = 0;r < m;r++){
        dp[0][r] = 1;
    }
    for(var j = 1;j<n;j++){
        for(var z = 1;z<m;z++){
            dp[j][z] = dp[j-1][z]+dp[j][z-1]
        }
    }
    return dp[n-1][m-1];
};
```
+ 优化版+1
  + 减少空间复杂度
  + 变为O(2n)
  + 分析
    + 由解法一可知：从左上角到右下角的走法 = 从右边开始走的路径总数+从下边开始走的路径总数 
    + 下一行的值 = 当前行的值+上一行的值 
    + dp[i][j] = dp[i-1][j]+dp[i][j-1]
    + <=> dp[j] = dp[j]+dp[j-1]
    + 此时的dp[j-1]代表上一阶段dp[j]的值
    + 即仅仅维护递推状态的最后两个状态
```javascript
/**
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
var uniquePaths = function(m, n) {
    var pre = new Array(n).fill(1);
    var cur = new Array(n).fill(1);
    for(var i = 1;i < m;i++){
        for(var r = 1;r < n;r++){
            cur[r] = cur[r-1] + pre[r];
        }
        pre = cur.slice(0);
    }
    return pre[n-1];
};
```
+ 优化版+2
  + 所以可以继续改写为
```javascript
/**
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
var uniquePaths = function(m, n) {
    var cur = new Array(n).fill(1);
    for(var i = 1;i < m;i++){
        for(var r = 1;r < n;r++){
            cur[r] = cur[r-1]+cur[r];
        }
    }
    return cur[n-1];
};
```
#### 解法二：排列组合
+ 组合思路来源
  + 如解法一
    + 从左上角到右下角的走法 = 从右边开始走的路径总数+从下边开始走的路径总数，和组合的定义道理是相通的：
    >组合的定义：从n个不同元素中，任取m(m≤n）个元素并成一组，叫做从n个不同元素中取出m个元素的一个组合；  
    从n个不同元素中取出m(m≤n）个元素的所有组合的个数，叫做从n个不同元素中取出m个元素的组合数。  
    用符号 C(n,m) 表示。  
    计算公式：  
    $$C_{n}^{m} = {{A_{n}^{m}} \over {m!}} = {{n!} \over {m!(n-m)!}}$$
    C(n,m) = C(n,n-m)，n >= m
  + 结合题意可知
    + 起点和终点不算在内，那总共走的步数为：N = m+n-2;
    + 向下走的步数为：k = m -1
    + 因此：
    + $$C_{n}^{k} = {{A_{n}^{k}} \over {k!}} = {{n!} \over {k!(n-k)!}}$$
    + $$化简为：{{{n*(n-1)*(n-2)*...(n-k+1)}} \over {k!}}$$
```javascript
/**
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
var uniquePaths = function(m, n) {
    var N = n+m-2;
    var k = m-1;
    var result = 1;
    for(var i = 1;i<= k;i++){
        result = result * (N-k+i)/i;
    }
    return result;
};
```
#### 解法三：暴力递归
+ 超时
```javascript
/**
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
var uniquePaths = function(m, n) {
    var row = n;
    var col = m;
    function helper(i,j){
        var tmp = 0;
        if(i == row -1 && j == col-1){
            return 1;
        }
        if(i >= row || j >= col){
            return 0;
        }
        tmp += helper(i+1,j);
        tmp += helper(i,j+1);
        return tmp;
    }
    return helper(0,0);
};
```