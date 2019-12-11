# 通配符匹配（困难）
# 题目描述
![截屏2019-12-10下午4.53.54.png](https://pic.leetcode-cn.com/ae8969f3ed499ac1ecca0c6cb41dd1fb9afb57c68235c7a1f1c5a383dcbf4dc1-%E6%88%AA%E5%B1%8F2019-12-10%E4%B8%8B%E5%8D%884.53.54.png)
![截屏2019-12-10下午4.54.03.png](https://pic.leetcode-cn.com/fec65ed871321845f37a8de651f2615c5a9c68e5ec0be1308143e43de146b1f3-%E6%88%AA%E5%B1%8F2019-12-10%E4%B8%8B%E5%8D%884.54.03.png)
# 题目地址
<https://leetcode-cn.com/problems/wildcard-matching/>
#### 解法一：动态规划
+ 类似题型
  + [10、正则表达式匹配-解法三](https://leetcode-cn.com/problems/regular-expression-matching/solution/10-zheng-ze-biao-da-shi-pi-pei-jiang-ti-jie-fu-zhi/)
+ 思路
  + 状态定义
    + dp[i][j]：表示 s 的前i个字符是否与 p的前j个字符是否匹配
  + 状态方程
    + **当 s[i] == p[j] 或者p[j] == '?' 时**
      + **dp[i][j] = dp[i-1][j-1]**
    + **当 s[i] != p[j] && p[j] == '*' 时**
      + **dp[i][j] = dp[i-1][j] || dp[i][j-1]**
      + dp[i-1][j]：匹配任意非空字符，例如abkk,ab*
      + dp[i][j-1]：匹配空字符相当于0个，例如ab，ab*
  + 初始化
    + dp[0][0]：两个空字符串，为true
    + dp[0][j]：当s为空，p为*号时，为true
      + p[j-1] == '*' && dp[0][j] = dp[0][j-1]
```javascript
/**
 * @param {string} s
 * @param {string} p
 * @return {boolean}
 */
var isMatch = function(s, p) {
    let n = s.length;
    let m = p.length;
    let dp = Array.from(new Array(n+1),() => new Array(m+1).fill(false));
    dp[0][0] = true;
   for(let j = 1;j <= m;j++){
       if(p[j-1] == '*'){
           dp[0][j] = dp[0][j-1];
       }
   }
    for(let i = 1;i <= n;i++){
        for(let j = 1;j <= m;j++){
            if(s[i-1] == p[j-1] || p[j-1] == '?'){
                dp[i][j] = dp[i-1][j-1];
            }else if(p[j-1] == '*'){
                 dp[i][j] = dp[i][j - 1] || dp[i - 1][j];
            }
        }
    }
    return dp[n][m];
};
```
#### 解法二：双指针
+ 时间复杂度：O(s*p)
+ 空间复杂度：O(1)
+ 思路
  + 和解法一类似，通过指针逐个儿匹配
    + 最后通过都匹配的指针位置与p的末尾位置比较，
      + 相同则说明s全部匹配
      + 否则，部分匹配不合题意
```javascript
/**
 * @param {string} s
 * @param {string} p
 * @return {boolean}
 */
var isMatch = function(s, p) {
    // 字符串s的起点位置
    let i = 0;
    // 字符串p的起点位置
    let j = 0;
    // s的当前位置
    let s_match = 0;
    // *的位置
    let startIdx = -1;
    let n = s.length;
    let m = p.length;
    while(i < n){
        // 同解法一：dp[i][j] = dp[i-1][j-1] 两指针同时后移
        if(j < m && (p[j] == '?' || s[i] == p[j])){
            i++;
            j++;
        }
        // 同解法一：dp[i][j] = dp[i][j-1] 
        // 匹配空串，更新*号的位置，更新s的当前位置，p后移
        else if(j < m && p[j] == '*'){
            startIdx = j;
            s_match = i;
            j++;
        }
        // 同解法一：dp[i][j] = dp[i-1][j]
        // 当前字符不匹配，并且也不是*号，回退到上一步
        // 匹配一个字符，例如abky,ab*z
        // j 回到 * 号的下一个位置
        // s_match继续下一个字符，i同样更新
        else if(startIdx != -1){
            j = startIdx + 1;
            s_match++;
            i = s_match;
        }
        // 字符不匹配，也没有 * 
        else{
            return false;
        }
    }
    // 将末尾如有，多余的 * 加上去，相当于匹配空串
    // ab，a*******
    while(j < m && p[j] == '*'){
        j++;
    }
    // 判断是否能够匹配全部 
    return j == m;
};
```