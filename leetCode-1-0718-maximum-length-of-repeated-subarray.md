# 最长重复子数组（中等）
# 题目描述
![截屏2020-02-05下午4.44.55.png](https://pic.leetcode-cn.com/2cca4564a6a984e3439c49dc80cb5f410754e9042f3a56db89c72535bf0f6d96-%E6%88%AA%E5%B1%8F2020-02-05%E4%B8%8B%E5%8D%884.44.55.png)
# 题目地址
<https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/>
#### 解法一：动态规划
+ 状态定义
  + dp[i][j]：为A[i:]和B[j:]最长公共前缀
+ 转移方程
  + A[i] == B[j]时
    + dp[i][j] = dp[i-1][j-1] + 1;
  + A[i] != B[j]时
    + dp[i][j] = 0;
```javascript
/**
 * @param {number[]} A
 * @param {number[]} B
 * @return {number}
 */
var findLength = function(A, B) {
    let resMax = 0;
    let n1 = A.length,n2 = B.length;
    let dp = Array.from(new Array(n1 + 1),() => new Array(n2 + 1).fill(0));
    for(let i = 1;i <= n1;i++){
        for(let j = 1;j <= n2;j++){
            if(A[i - 1] === B[j - 1]){
                dp[i][j] = dp[i-1][j-1] + 1;
                resMax = Math.max(resMax,dp[i][j]);
            }
        }
    }
    return resMax;
};
```
#### 解法二：遍历矩阵
+ [参考这里](https://leetcode.com/problems/maximum-length-of-repeated-subarray/discuss/109040/Java-O(mn)-time-O(1)-space)
```javascript
/**
 * @param {number[]} A
 * @param {number[]} B
 * @return {number}
 */
var findLength = function(A, B) {
    let resMax = 0,match = 0;
    let n1 = A.length,n2 = B.length;
    for(let j = 0;j < n2;j++){
        match = 0;
        for(let i = 0,k = j;i < n1 && k < n2;i++,k++){
            if(A[i] != B[k]) match = 0;
            else{
                match++;
                resMax = Math.max(resMax,match);
            }
        }
    }
    for(let i = 1;i < n1;i++){
        match = 0;
        for(let j = 0,k = i;k < n1 && j < n2;j++,k++){
            if(A[k] != B[j]) match = 0;
            else{
                match++;
                resMax = Math.max(resMax,match);
            }
        }
    }
    return resMax;
};
```