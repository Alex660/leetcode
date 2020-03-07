# 完全平方数（中等）
# 题目描述
![截屏2020-03-03下午2.41.50.png](https://pic.leetcode-cn.com/3a6ecaafcc514c9347040e88e58e3cc6906c33b23e5ce1f20299337f6fb96ffe-%E6%88%AA%E5%B1%8F2020-03-03%E4%B8%8B%E5%8D%882.41.50.png)
# 题目地址
<https://leetcode-cn.com/problems/perfect-squares/>
#### 各类算法模板
+ [BFS](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/AlgorithmTemplate%E7%AE%97%E6%B3%95%E6%A8%A1%E6%9D%BF.md)
#### 解法一：动态规划
+ 时间复杂度：O(n$\sqrt{n}$)
+ 空间复杂度：O(n)
+ 思路
  + 状态定义：```dp[i]```：表示当前数字```i```最少有几个平方数构成
  + 转移方程：```dp[i] = min(dp[i],dp[i - j*j] + 1)```
```javascript
/**
 * @param {number} n
 * @return {number}
 */
var numSquares = function(n) {
    let dp = new Array(n+1).fill(0);
    for(let i = 1;i <= n;i++){
        dp[i] = i;
        for(let j = 1;j*j <= i;j++){
            dp[i] = Math.min(dp[i],dp[i - j*j] + 1);
        }
    }
    return dp[n];
};
```
#### 解法二：BFS
```javascript
/**
 * @param {number} n
 * @return {number}
 */
var numSquares = function(n) {
    let queue = [n];
    let visited = {};
    let level = 0;
    while(queue.length > 0) {
        // 层序遍历
        level++;
        let len = queue.length;
        for(let i = 0;i < len;i++){
            let cur = queue.pop();
            for(let j = 1;j*j <= cur;j++){
                let tmp = cur - j*j;
                // 找到答案
                if(tmp === 0) {
                    return level;
                }
                if(!visited[tmp]){
                    queue.unshift(tmp);               
                    visited[tmp] = true;
                }
            }
        }
    }
    return level;
};
```
#### 解法三：拉格朗日四平方和定理
+ 定义
  + 每个正整数均可表示为4个整数的平方和。
  + 它是费马多边形数定理和华林问题的特例。
+ 参考文献
  + [拉格朗日四平方和定理证明](https://zhuanlan.zhihu.com/p/104030654)
```javascript
/**
 * @param {number} n
 * @return {number}
 */
var numSquares = function(n) {
    if(Math.pow(Math.floor(Math.sqrt(n)),2) === n) return 1;
    while(n%4 === 0){
        n = n/4;
    }
    if((n-7)%8 === 0){
        return 4;
    }
    for(let y,x = 1;x*x < n;x++){
        y = Math.floor(Math.sqrt(n - x*x));
        if(x*x + y*y === n) return 2;
    }
    return 3;
};
```
+ 位运算进阶版
```javascript
/**
 * @param {number} n
 * @return {number}
 */
var numSquares = function(n) {
    if(Math.pow(Math.floor(Math.sqrt(n)),2) === n) return 1;
    while((n & 3) === 0){
        n  >>= 2;
    }
    if((n & 7) === 7){
        return 4;
    }
    for(let y,x = 1;x*x < n;x++){
        y = Math.floor(Math.sqrt(n - x*x));
        if(x*x + y*y === n) return 2;
    }
    return 3;
};
```