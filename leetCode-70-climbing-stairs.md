# 爬楼梯（简单）
# 题目描述
![截屏2019-10-20上午9.16.53.png](https://pic.leetcode-cn.com/2baa6fa9ccb8cd29e761f3995cb201be418f63749c862f409a28e4b1273b5830-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.16.53.png)
# 题目地址
<https://leetcode-cn.com/problems/climbing-stairs/solution/>
# 解法一
![截屏2019-10-17上午9.43.43.png](https://pic.leetcode-cn.com/71ab6e7d7b0681855a67b8c18ce491bc888e9f12dd73d1e11886aedc889d0058-%E6%88%AA%E5%B1%8F2019-10-17%E4%B8%8A%E5%8D%889.43.43.png)
# 解法二公式
![截屏2019-10-17上午11.11.52.png](https://pic.leetcode-cn.com/ad346fde8d24b738be88554eb02477bbe2de3cbdd41a37fe12a70219f69e5d3d-%E6%88%AA%E5%B1%8F2019-10-17%E4%B8%8A%E5%8D%8811.11.52.png)

> 爬楼梯
 爬楼梯设需要n阶才能到达楼顶，每次可以爬 1 或 2 个 台阶，问有多少种不同的方法才能爬到楼顶？
+ input 2 output 2  1+1、2
+ input 3 output 3  1+1+1、1+2、2+1

#### 解法一：<=> 求第i个斐波那契数
+  维护3个变量 每次递归更新前两个子问题所需步数
+  可知递推公式 == f(n)  = f(n-2) + f(n-1),n>=1
+ 1  1  2  3  5  8  13  21
+ 时间复杂度：O(n)
+ 空间复杂度：O(1)
+ 代码
```javascript 
/**
* @param {number} n
* @return {number}
*/
var climbStairs = function(n) {
    var f1 = 2;
    var f2 = 3;
    var f3 = 0;
    if(n <= 3){
        return n;
    }
    while(n>3){
        f3 = f2 + f1;
        f1 = f2;
        f2 = f3;
        n--;
    }
    return f2;
};
```    

#### 解法二：由二阶递推 可以得出 斐波那契数列的 特征方程为 x^2 = x^1 +1;
+ 由数学证明公式可得代码如下
```javascript
/**
* @param {number} n
* @return {number}
*/
var climbStairs = function(n) {
    var sqrt5 = Math.sqrt(5);
    var pow =  Math.pow((1+sqrt5)/2,n+1) - Math.pow((1-sqrt5)/2,n+1);
    return Math.round( pow/sqrt5 );
};
```

#### 解法三：动态规划
+ 时间复杂度:O(n)
+ 空间复杂度:O(n)
+ 符合：
    + 最优子结构(子问题的最优解推出问题的最优解)
    + 重复子问题(递归求解过程成会重复计算之前已经计算过的问题)
    + 无后效性(前面的状态一旦确定不会因为后面的状态改变而改变)
+ 代码
```javascript
/**
* @param {number} n
* @return {number}
*/
var climbStairs = function(n) {
    // 求第n步 所以索引到n
    var dp = new Array(n+1);
    if(n <= 3){
        return n;
    }
    dp[1] = 1;
    dp[2] = 2;
    for(var i = 3;i<=n;i++){
        dp[i] = dp[i-2] + dp[i-1];
    }
    return dp[n];
};
```

#### 解法四：暴力求解
```javascript
/**
* @param {number} n
* @return {number}
*/
var climbStairs = function(n) {
    var climb = function(i,n){
        if(i > n){
            return 0;
        }
        if(i == n){
            return 1;
        }
        return  climb(i+1,n) + climb(i+2,n);
    }
    return climb(0,n);
};
``` 
#### 解法五：解法一的简化版
+ 分析：同样不断更新前后两个数值
```javascript
/**
* @param {number} n
* @return {number}
*/
var climbStairs = function(n) {
    var a = 1;
    var b = 1;
    while(n--){
        a = (b+=a) -a;
    }
    return a;
};
```