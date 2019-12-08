# 最长公共子序列（中等）
# 题目描述
![截屏2019-11-13上午9.56.19.png](https://pic.leetcode-cn.com/0644e3abf93f32400cd7e12497c0a7d25156cb3582e6281180ed95d62ed49b13-%E6%88%AA%E5%B1%8F2019-11-13%E4%B8%8A%E5%8D%889.56.19.png)
![截屏2019-11-13上午9.56.27.png](https://pic.leetcode-cn.com/100b941ef7423ceeee23a0694ea3dd92b42fb932a38fcd927a01af97bed43e87-%E6%88%AA%E5%B1%8F2019-11-13%E4%B8%8A%E5%8D%889.56.27.png)
# 题目地址
<https://leetcode-cn.com/problems/longest-common-subsequence/>
#### 解法：动态规划
+ 思路分析
  + 图解
   ![截屏2019-11-13下午4.40.21.png](https://pic.leetcode-cn.com/b0bd1ccee5ff5e66fa78ae11bd47d8835f94aa94e5cb5c6d7457ac24bdb3ae95-%E6%88%AA%E5%B1%8F2019-11-13%E4%B8%8B%E5%8D%884.40.21.png)
  + 数学归纳法（***以下dp{str1,str2}代表对str1和str2求最长子序列***)
    + 1、text1 = "任意字符串"、 text2 = "任意字符串"
      + text1为**单个**字符，则所求为text1
        + 如图所示，紫色字母
          + text1：B
          + text2：A、AB、ABA、ABAZ、ABAZD、ABAZDC
          + 最长即为1
      + text1为**多个**字符，则
        + text1 = "......A"、text2= "..A/......A"
          + 末尾字符相同
          + 则所求最长公共子序列为
            + dp{ text1[n-1] , text2[n-1] } + 1
            + 最后一个字符相同，则最长公共子序列一定有这个数
          + 如图所示，左边笑脸和上面笑脸处
            + text1：BAC
            + text2：ABAZDC
            + 最长即为：dp{ text1[B~A] , text2[A~D] } + 1 = 2 +1 = 3
        + text1 = "......A"、text2= "......B"
          + 末尾字符不相同
          + 则所求最长公共子序列为
            + Math.max( dp{ text1[n-1] , text2[n] } , dp{ text1[n] , text2[n-1] } )
          + 如图所示
            + text1：BAC
            + text2：ABA
            + 最长即为：Math.max( dp{ text1[B~A] , text2[A~A] } , dp{ text1[B~C] , text2[A~B] } ) = Math.max(2,1) = 2
    + 2、归纳公式
      + 设 s1 = text1
      + s2 = text2
      + row = s1.length
      + col = s2.length
      + dp[s1,s2]为求接方程
      + 当 **s1[row-1] != s2[row-1]** 时
        + **dp[s1,s2] = Max( dp[s1-1,s2] , dp[s1,s2-1] )**
        + 或者，考虑末尾字符都不要的情况
        + dp[s1,s2] = Max( dp[s1-1,s2] , dp[s1,s2-1] , dp[s1-1,s2-1] )
          + 即要么两个字符串都不考虑最后一个字符，要么其中一个考虑最后一个字符
          + 事实上前面dp[s1-1,s2]求解子问题时候，会包含dp[s1-1,s2-1]，且最后一种末尾字符相同的情况下也包含此子问题
          + 所以按照第一个dp[s1,s2]方程来即可。
      + 当 **s1[row-1] == s2[row-1]** 时
        + **dp[s1,s2] = dp[s1-1,s2-1] + 1**
        + 或者，考虑剩余所以可能子情况
        + dp[s1,s2] = Max( dp[s1-1,s2] , dp[s1,s2-1] , dp[s1-1,s2-1] , (dp[s1-1,s2-1] + 1) )
          + 事实上，由以上分析可知
            + **最后一个字符相同，则最长公共子序列一定有这个数**
            + 但是如果不考虑最后一个字符或者其中一个少考虑最后一个字符，
              + 意味着求出的最长公共子序列可能会少一个
              + 所以Max()里最大的一定是(dp[s1-1,s2-1] + 1)这种情况，即只需要第一个dp[s1,s2]方程即可。
+ 类似题型
  + [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/solution/62-bu-tong-lu-jing-by-alexer-660/) 
  + [63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/solution/63-bu-tong-lu-jing-ii-by-alexer-660/)
  + [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/solution/70-pa-lou-ti-by-alexer-660/)
```javascript
/**
 * @param {string} text1
 * @param {string} text2
 * @return {number}
 */
var longestCommonSubsequence = function(text1, text2) {
    let n = text1.length;
    let m = text2.length;
    let dp = Array.from(new Array(n+1),() => new Array(m+1).fill(0));
    for(let i = 1;i <= n;i++){
        for(let j = 1;j <= m;j++){
            if(text1[i-1] == text2[j-1]){
                dp[i][j] = dp[i-1][j-1] + 1;
            }else{
                dp[i][j] = Math.max(dp[i][j-1],dp[i-1][j]);
            }
        }
    }
    return dp[n][m];
};
```