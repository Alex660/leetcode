# 解码方法（中等）
# 题目描述
![截屏2019-12-04下午5.30.13.png](https://pic.leetcode-cn.com/9b26b8af02c78e7297564733e4a827de5cc3bc5e5eb3e0101a7f2e30086c0a84-%E6%88%AA%E5%B1%8F2019-12-04%E4%B8%8B%E5%8D%885.30.13.png)
# 题目地址
<https://leetcode-cn.com/problems/decode-ways/>
#### 解法一：动态规划
+ 思路
    >'A' -> 1  
    'B' -> 2  
    ...  
    'Z' -> 26
    + 由示例和题中转码公式可知
      + 数字转换为字母，最大支持26，最小支持1：
        + 数字组合**最大为两位数，且小于27，大于0**
          + 当为1位数时，形如**X**
            + 0 
              + 首字母，返回0
              + 非首字母，跳过，此处组合为0
            + 非0
              + 本身为1个组合
            + **转移方程**
              + **s[i-1] != 0 && dp[i] = dp[i-1] + dp[i]**
          + 当为2位数时，形如**nX**
            + 1X
              + 10、11、12...19 所以X可以是任意值
            + **转移方程**
              + **s[i-2] == 1 && dp[i] = dp[i] + dp[i-2]**
            + 2X
              + 20、21、21...26 所以 X < 7
            + **转移方程**
              + **s[i-2] == 2 && s[i-1] < 7 && dp[i] = dp[i] + dp[i-2]** 
            + 3X、4X、5X、6X、7X、8X、9X
              + 30、40、50、60、70、80、90 
              + 所以 n > 3 时，X只能为0，可以直接跳过
              + 因此可以归纳到第一种情况中去，为1位数：形如**X**，直接跳过
        + 数字为 0 时，无法解码
          + 又根据题意，首数字必须可以解码，如果数字第一位为0，则解码总数为0，视为无效
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var numDecodings = function(s) {
    if(s[0] == 0){
        return 0;
    }
    let n = s.length;
    let dp = new Array(n+1).fill(0);
    dp[0] = dp[1] = 1;
    for(let i = 2;i <= n;i++){
        if(s[i-1] != 0){
            dp[i] += dp[i-1];
        }
        if((s[i-2] == 1) || (s[i-2] == 2 && s[i-1] <= 6)){
            dp[i] += dp[i-2];
        }
    }
    return dp[n];
};
```
#### 解法二：递归
+ 通过索引 穷举 i-2和i-1的所有可能组合情况
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var numDecodings = function(s) {
    if(s[0] == 0){
        return 0;
    }
    let n = s.length;
    let helper = (start) => {
        if(start == n){
            return 1;
        }
        if(s[start] == 0){
            return 0;
        }
        let odd = helper(start+1);
        let even = 0;
        if(start < n - 1){
            let ten = s[start];
            let one = s[start+1];
            if((ten+''+one) < 27){
                even = helper(start+2);
            }
        }
        return odd + even;
    }
    return helper(0);
};
```
#### 解法三：递归 + 备忘录
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var numDecodings = function(s) {
    if(s[0] == 0){
        return 0;
    }
    let n = s.length;
    let memo = new Map();
    let helper = (start) => {
        if(start == n){
            return 1;
        }
        if(s[start] == 0){
            return 0;
        }
        let memoVal = memo.get(start);
        if(memoVal){
            return memoVal;
        }
        let odd = helper(start+1,memo);
        let even = 0;
        if(start < n - 1){
            let ten = s[start];
            let one = s[start+1];
            if((ten+''+one) < 27){
                even = helper(start+2,memo);
            }
        }
        let res = odd + even;
        memo.set(start,res);
        return res;
    }
    return helper(0,memo);
};
```