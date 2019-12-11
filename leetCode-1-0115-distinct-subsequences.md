# 不同的子序列（困难）
# 题目描述
![截屏2019-12-10下午4.20.17.png](https://pic.leetcode-cn.com/d6625102d9a93f9626662cbfdde11a373b81a13d689256ab3abda1d2f6027c13-%E6%88%AA%E5%B1%8F2019-12-10%E4%B8%8B%E5%8D%884.20.17.png)
![截屏2019-12-10下午4.20.24.png](https://pic.leetcode-cn.com/ade5c9f6ac42c9483fb30a30f3d40f9fb4814b7fa03e9f51175bfabeed19393f-%E6%88%AA%E5%B1%8F2019-12-10%E4%B8%8B%E5%8D%884.20.24.png)
# 题目地址
<https://leetcode-cn.com/problems/distinct-subsequences/>
#### 解法：动态规划
+ 类似题型
  + [72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/solution/72-bian-ji-ju-chi-by-alexer-660/)
+ 思路
  + 状态定义
    + dp[i][j]：代表 T 前 i 字符串可以由 S 前 j 字符串组成最多个数
    + 定义缘由
      + 由题意可知，从 S 中可以找出 n个 T字符串，通过摘取 S中n个字符组成，且这n个字符取出的顺序要和 T相同，且绝对位置不能变即从左到右，可以跳着取
      + S 字符串的长度 大于等于 T字符串
        + **说明 S 中包含 n 个 T子序列**
      + **当T长度为1时，最小子序列为一个字符，此为递推关键**
      + 可以从S中**删除或不删除n个字符**，以拼接为T的样子
    + 状态方程
      + 那么从1开始遍历 T
        + 当 **T[i] == S[j]** 时
          + **dp[i][j] = dp[i-1][j-1]**
            + 说明当前位置的两个字符相同，所以两边同时可以减少1位继续比较
            + 相当于同时删除一个相同最小子序列字符
          + **dp[i][j] = dp[i][j-1]** 
            + 或者将大串S减少一位继续比较，相当于子序列个数少算一个
            + 因为S 中可以包含 n 个 T子序列，那么倒退回去也是正确的，直到只剩一个子序列
            + 相当于S 删除一个不同的最小子序列字符
        + 当 **T[i] != S[j]** 时
          + **dp[i][j] = dp[i][j-1]**
            + 同上，S中前i位置不符合，则说明要继续后退，直到上面找到一个相同的子序列
            + 相当于S 删除一个不同的最小子序列字符
    + 初始化
      + 当T为空时，S中无论有多少个字符
        + 无论遍历或者从0到i位置中可能截取几个字符，只能都删除掉成为空，才能与空字符表示匹配
        + 子序列个数为1
        + **此为递推起点值**
```javascript
/**
 * @param {string} s
 * @param {string} t
 * @return {number}
 */
var numDistinct = function(s, t) {
    let n = s.length;
    let m = t.length;
    let dp = Array.from(new Array(m+1),() => new Array(n+1).fill(0));
    for(let i = 0;i <= n;i++){
        dp[0][i] = 1;
    }
    for(let i = 1;i <= m;i++){
        for(let j = 1;j <= n;j++){
            if(t[i-1] == s[j-1]){
                dp[i][j] = dp[i][j-1] + dp[i-1][j-1];
            }else{
                dp[i][j] = dp[i][j-1];
            }
        }
    }
    return dp[m][n];
};
```