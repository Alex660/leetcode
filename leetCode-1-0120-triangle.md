# 三角形最小路径和（中等）
# 题目描述
![截屏2019-11-14上午9.25.03.png](https://pic.leetcode-cn.com/c1df64435c56840f4c53e99361de602470c248403d8c89f881383d1857be07b6-%E6%88%AA%E5%B1%8F2019-11-14%E4%B8%8A%E5%8D%889.25.03.png)
# 题目地址
<https://leetcode-cn.com/problems/triangle/description/>
#### 解法一：动态规划-自底向上
+ 类似题型
  + [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/solution/62-bu-tong-lu-jing-by-alexer-660/) 
  + [63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/solution/63-bu-tong-lu-jing-ii-by-alexer-660/)
+ 图解
  + ![截屏2019-11-14上午10.34.25.png](https://pic.leetcode-cn.com/2c80c2bc9cac51261414a86eb49427528041383d2e32ae58782cb81cfbdcc54d-%E6%88%AA%E5%B1%8F2019-11-14%E4%B8%8A%E5%8D%8810.34.25.png)
+ DP
  1. 重复性(分治) 
     1. problem(i,j) = min(sub(i+1,j) , sub(i+1,j+1)) + a[i,j]
        1. problem(i,j)：当前行当前列（二维数组）的向下面走的路径总数
        2. sub(i+1,j)：下一行当前列(即向下并向左边走)的路径总数
        3. sub(i+1,j+1)：下一行下一列(即向下并向右边走)的路径总数
        4. 路径总数包括自己所在位置a[i,j]，即到达当前位置所需的步数
  2. 定义状态数组
        1. dp[i,j]
  3. DP方程
        1. dp[i,j] = min(dp[i+1,j],dp[i+1][j+1])+dp[i,j]
  4. 初始化数据
     1. 一般是第一行n列和第一列n行**或者**最后一行n列最后一列n行
     2. 但题中本意就是为了比较相邻数字和的大小，直接用原题的数据，最后一行n列即可对推到起点。
```javascript
/**
 * @param {number[][]} triangle
 * @return {number}
 */
var minimumTotal = function(triangle) {
    var dp = triangle;
    for(var i = dp.length-2;i >= 0;i--){
        for(var j = 0;j < dp[i].length;j++){
            dp[i][j] = Math.min(dp[i+1][j],dp[i+1][j+1]) + dp[i][j];
        }
    }
    return dp[0][0];
};
```
+ 优化版+1
  + 题目说明尽可能减少空间复杂度
  + 暂且不考虑项目影响只针对此题目并由上面代码可知
  + 无需另外开一个dp数组，直接复用即可
```javascript
/**
 * @param {number[][]} triangle
 * @return {number}
 */
var minimumTotal = function(triangle) {
    for(var i = triangle.length-2;i >= 0;i--){
        for(var j = 0;j < triangle[i].length;j++){
            triangle[i][j] = Math.min(triangle[i+1][j],triangle[i+1][j+1]) + triangle[i][j];
        }
    }
    return triangle[0][0];
};
```
#### 解法二：动态规划-自底向上-降维
+ 空间复杂度：O(n)
+ 自底向上递归时，其实每次只会用到上一层数据，因此不需二维数组存储所有可能情况来一一比较
```javascript
/**
 * @param {number[][]} triangle
 * @return {number}
 */
var minimumTotal = function(triangle) {
    var dp = new Array(triangle.length+1).fill(0);
    for(var i = triangle.length-1;i >= 0;i--){
        for(var j = 0;j < triangle[i].length;j++){
            dp[j] = Math.min(dp[j],dp[j+1]) + triangle[i][j];
        }
    }
    return dp[0];
};
```
#### 解法三：递归，自顶向下
+ 超时
```javascript
/**
 * @param {number[][]} triangle
 * @return {number}
 */
var minimumTotal = function(triangle) {
    var len =triangle.length;
    var row = 0;
    var col = 0;
    function helper(row,col,triangle){
        // terminator
        if(row == len -1){
            return triangle[row][col];
        }
        // drill down
        var left = helper(row+1,col,triangle);
        var right = helper(row+1,col+1,triangle);
        return Math.min(left,right)+triangle[row][col];
    }
    return helper(row,col,triangle);
};
```
#### 解法四：递归，自顶向下、备忘录
+ 超时
```javascript
/**
 * @param {number[][]} triangle
 * @return {number}
 */
var minimumTotal = function(triangle) {
    var len =triangle.length;
    var row = 0;
    var col = 0;
    var memo = new Array(row);
    for(var i = 0;i < len;i++){
        // 由题可知每行最大列数即为行数
        memo[i] = new Array(row);
    }
    function helper(row,col,triangle){
        // terminator
        if(row == len -1){
            return memo[row][col] = triangle[row][col];
        }
        // drill down
        var left = helper(row+1,col,triangle);
        var right = helper(row+1,col+1,triangle);
        return Math.min(left,right)+triangle[row][col];
    }
    return helper(row,col,triangle);
};
```