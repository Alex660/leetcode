# 正则表达式匹配（困难）
# 题目描述
![截屏2019-10-20上午9.24.16.png](https://pic.leetcode-cn.com/4329cfa5d3973bbc2416415f4c9db5c7cf5f6777300e6d93a9eb8dd0b0674977-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.24.16.png)
![截屏2019-10-20上午9.24.28.png](https://pic.leetcode-cn.com/3110b8c4a41a6282dcadd2b4447a2de5113166a97cd9d766a10484bc1c189132-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.24.28.png)
![截屏2019-10-20上午9.24.34.png](https://pic.leetcode-cn.com/5862835f47b554f7bd9a6759037b1707481caf0c7d64105f45a8e77012f179b8-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.24.34.png)
# 题目地址
<https://leetcode-cn.com/problems/regular-expression-matching/solution/>
# 解法二
![截屏2019-10-14下午4.46.08.png](https://pic.leetcode-cn.com/3c0dcaf4ebfaa08aad10052bea7e73f33a68e19638b9074c2489d792ebef96dc-%E6%88%AA%E5%B1%8F2019-10-14%E4%B8%8B%E5%8D%884.46.08.png)
# 解法一
![截屏2019-10-14下午4.48.19.png](https://pic.leetcode-cn.com/7c0cdbe3ed1d23ee8ace2c148baaebd7edf05f8f9c1af2fef361b395c2bc16e4-%E6%88%AA%E5%B1%8F2019-10-14%E4%B8%8B%E5%8D%884.48.19.png)
>实现简单规则匹配：  
1、  '.'代表任意单个字符  
2、  '*'代表前一字符 重复 0次 或 任意次
#### 解法一：递归回溯 ***这里的回溯不是回溯算法！***

+ 假设仅可能出现 '.' 字符 ，不会出现 '*' 字符
    + 设匹配函数：isMatch 比较 text 和 pattern 是否匹配 boolean isMatch (String text,String pattern)

    + 递归分析 =>

    + text 和 pattern 匹配 

    + <= text 和 pattern 的第一个字符都匹配 && 两边剩余的字符也都匹配 
    + <= text 和 pattern 的第一个字符都匹配 && 两边剩余的字符的第一个字符都匹配 && 两边剩余的字符都匹配
    + <= ...
    + <= text 和 pattern 的第一个字符都匹配 && ... && ... && ... && 两边剩余的最后一个字符都匹配

    + <= pattern 为空时 ： text为空 ? true : false
    
         if(pattern.isempty()) return text.isEmpty();


    + ( (pattern.charAt(0) == text.charAt(0) || pattern.charSt(0) == '.' ) &&
    isMatch( text.substring(1),pattern.substring(1) )


+ 所以不考虑 '*' 匹配规则时 =>
```javascript
function isMatch(text,pattern){
    if(pattern == '' || !pattren){
        return text == '';
    }
    // 判断 text 是否为空 防止越界，如果 text 为空，表达式直接为 false , text.charAt(0) 就不会执行了
    var first_match = (!text && (pattern.charAt(0) == text.charAt(0) || pattern.charAt(0) == '.' );
    return first_match && isMatch(text.substing(1),pattern.substring(1)));

}
```

+ 当考虑 '*' 匹配规则时 =>
```javascript
/**
* @param {string} s
* @param {string} p
* @return {boolean}
*/
var isMatch = function(s, p) {
    if(p == '' || !p){
        return s == '';
    }
    // 判断 s 是否为空 防止越界，如果 s 为空，表达式直接为 false , s.charAt(0) 就不会执行了
    var first_match = (s && (p.charAt(0) == s.charAt(0) || p.charAt(0) == '.' ));
    // 当长度大于2的时候，才考虑 *
    // ---两种情况
        //p 直接跳过两个字符，表示  * 前边的字符出现 0 次
        //p 不变，接着用第一个字符和前面比较，表示 * 用前一个字符替代 【s = aa , p = a*】
    if(p.length >= 2 && p.charAt(1) == '*'){
        return (
            isMatch(s,p.substring(2)) ||
            (first_match && isMatch(s.substring(1),p))
        )
    }else {
        return first_match && isMatch(s.substring(1),p.substring(1));
    }
};
```
#### 解法二：动态规划 A
>正则表达式匹配  
给定字符串 s  一个字符串匹配规则 p  
实现一个支持 '.' 和'*'匹配规则的正则表达式匹配  
'.' 匹配前面 任意单个 字符  
'*' 匹配前面 零个或多个(任意个) 前面的那一个元素
+ （一）、状态
    +  设待字符串s、匹配规则串p
    +  设dp[i][j] 为s[0-i] 与 p[i-j] 即 p的前j个字符串完全匹配s的前i个字符串
    +  二维数组表示状态
+  (二)、状态转移方程
    +  倒推：【所以以i-1,j-1为前一位基准索引】
    +  dp[i][j] == true  => dp[i-1][j-1] == true => ... => dp[0][0] == true
    +  dp[i][j] == true  <= s[i]和p[j]符合匹配规则 && dp[i-1][j-1] == true
    +  如果没有特殊匹配规则即完全字符与字符之间的比较则 dp[i][j] == true  <=>  dp[i-1][j-1] == true && s[i] == p[j]
    +  如果有特殊的字符如题'.','*'，则会产生特殊情况
    +  i,j从1 开始遍历 ,因为是倒推 
    +  设
    +  s当前遍历字符：curS=s[i-1],
    +  p当前遍历字符：curP=p[j-1],
    +  p当前遍历字符上一位：preCurP = p[j-2]
        + 没有遇到特殊字符匹配规则
            + 当且仅当 curS==curP 时  dp[i][j] = dp[i-1][j-1] ---- <1>
                >例如：  
                ####a i==a  
                ######a j==a     
        + 遇到了特殊字符匹配规则
            + 遇到'.'时 
              + 当且仅当 curP=='.' 时，dp[i][j] = dp[i-1][j-1] ---- <2>
                >例如：  
                ####b i==b   
                ####. j==.
            + 遇到'*'时
                + '*'前一位没有遇到特殊字符匹配规则
                    + 当且仅当 preCurP != '.' && preCurP != '*' 时
                        > 例如:  
                        ####Y{1,} i == Y  
                        ###X{1,}* j == * ,X != '.'
                        + 当 X == Y  <=> preCurP == curS 时
                            + X==1个时，dp[i][j] = dp[i-1][j-2] ---- <3>
                                >例如：  
                                ####b i == b,Y == b  
                                ###b* j == *,X == b
                            + X>1个时，dp[i][j] = dp[i-1][j] ---- <4> 
                            + 即递归到第一种情况 X==1个
                                >例如：  
                                ####bb  i == b,Y == bb  
                                ####bbb i == b,Y == bbb  
                                ...  
                                ###b*   j == *,X == b
                        + 当 X!=Y <=> preCurP != curS 时，dp[i][j] = dp[i][j-2] ---- <5>
                            >例如：  
                            ####b i == b == Y  
                            ###c* j == *,X != b
                        +  即 c* 匹配为0次 => j-2
                + '*'前一位遇到特殊字符匹配规则
                    + 当且仅当 preCurP == '.'时
                        >例如：  
                        ####Y{1,} i == Y  
                        ###{1,}.* j == * ,preCurP == '.' 
                        +  当 . 替换成 Y 时
                            + 当 Y == 0个即 * 不匹配Y时
                                + <=> 第<5>种情况 即 dp[i][j] = dp[i][j-2] ---- <6>
                            + 当 Y == 1个即 * 匹配1个时
                                + '.*' 中 * 匹配前面字符Y 1个，.替换为 curS  总体匹配了一次('.*'匹配b一次 即 '.*' = b) curS => dp[i][j] = dp[i-1][j-2] ---- <7>
                                    >例如：  
                                    ####b  i == b  
                                    ###.*  j == *,preCurP == '.'
                            + 当 Y>1 个 即 * 匹配多个Y时，dp[i][j] = dp[i-1][j] ---- <8>
                                + 即递归到第一种情况 X==1个
                                    >例如：  
                                    ####bb  i == b  
                                    ####bbb i == b  
                                    ...  
                                    ###.*   j == *,preCurP == '.'

                    + 当且仅当 preCurP == '*' 
                    + 此种情况不可能存在 因为 '*'表示匹配规则本身不能代表或等价于任何字符只能匹配前一个字符
                    + 但是'..'可以,因为'.'本身可以代表任何单个字符 也是一种匹配规则
____
+ 由上可知合并分析为核心操作：
    ```javascript
    if(curP == '.' || curS == curP){
        // 合并<1>、<2>步
        dp[i][j] = dp[i-1][j-1];
    }else if(curP == '*'){
        // <5>步两重限制下只有一种情况【preCurP== '*'时不存在，且会报非法情况报语法错误 】
        if(preCurP != '.'  && preCurP != curS){
            dp[i][j] = dp[i][j-2]
        }
        // 合并<3><4><6><7><8>步，因为不在上述情况内
        else{
            dp[i][j] = dp[i-1][j-2] || dp[i-1][j] || dp[i][j-2]
        }
    }
    ```
___ 
+  (三)、第一个状态为特殊数据可设置为空串' '【哨兵优化模式】 && 初始化第一行和第一列数据【画状态转移表如图】 => 初始化二维数组
    +  dp[0][0] = true
    +  均是匹配空串：[匹配' '除了 ' ' 本身，还有即使X*时 j-2为真的话才能逆向推导为真]
        + 初始化第0行第j列数据 p[j]
            + j-2>=0 && p[j-1]=='*' && dp[0][j] = dp[0][j-2]
        + 初始化第i行第0列数据 p[0] == ''
            + 因为空串只能匹配空串 所以均为false => dp[i][0] = false
            
+  (四)、结果
    +  dp[i][j] 真假即为所求

+  (五)、理解
    +  dp[i-1][j] 意为 s前i-1个字符符合匹配p
    +  dp[i][j-1] 意为 s符合p前j-1个字符匹配规则 
    +  dp[i][j-2] 意为 跳过当前字符的前一个字符去匹配前两个字符 匹配0次或重复1次时就可跳过
    +  ...

+ 正则真解 【题外话】
 + '.*' 为 贪婪匹配模式 如  101000000000100 匹配 '1.*1'  结果为 1010000000001
 +  '.?'为 懒惰模式 只匹配满足条件的一次 上面会匹配 '1.?' 结果为 101
```javascript
/**
 * @param {string} s
 * @param {string} p
 * @return {boolean}
 */
var isMatch = function(s, p) {
    if(s==null || p==null){
        return false;
    }
    var m = s.length+1;
    var n = p.length+1;
    // 初始化二维状态数组 && 初始化第i行第0列数据
    var dp = new Array(m);
    for(var i = 0;i<m;i++){
        dp[i] = new Array(n)
        for(var j = 0;j<n;j++){
            dp[i][j] = false;
        }
    }
    // 初始化第一个状态
    dp[0][0] = true;
    // 初始化第0行j列数据
    for(var j=2;j<n;j++){
        if(p[j-1] == '*'){
            dp[0][j] = dp[0][j-2];
        }
    }
    // 动态转移表法
    var curS = 0;//当前待匹配字符串i字符位置
    var curP = 0;//当前匹配规则字符串j字符位置
    var preCurP = 0;//当前匹配规则字符串j字符前一个位置
    for(var i =1;i<m;i++){
        for(var j = 1;j<n;j++){
            // 从1开始遍历
            curS = s[i-1];
            curP = p[j-1];
            preCurP = p[j-2];
            if(curP == '.' || curS == curP){
                // 合并<1>、<2>步
                dp[i][j] = dp[i-1][j-1];
            }else if(curP == '*'){
                // <5>步两重限制下只有一种情况【preCurP== '*'时不存在，且会报非法情况报语法错误 】
                if(preCurP != '.'  && preCurP != curS){
                    dp[i][j] = dp[i][j-2]
                }
                // 合并<3><4><6><7><8>步，因为不在上述情况内
                else{
                    dp[i][j] = dp[i-1][j-2] || dp[i-1][j] || dp[i][j-2]
                }
            }
        }
    }
    return dp[m-1][n-1];
}
```
#### 解法三：动态规划 B
+ 思路分析
  + 状态定义
    + dp[i][j]：表示s的前i个是否能被p的前j个匹配，布尔类型
  + 状态方程
    + **p[j] == s[i]**
      + **dp[i][j] = dp[i-1][j-1]**
      + 当前同一位置的字符匹配，则判断上一位的字符是否匹配
    + **p[j] == "."**
      + **dp[i][j] = dp[i-1][j-1]**
      + "."可以换成和s[i]相同的字符，则情况同第一种一样
    + **p[j] == " * "**
      + " * "匹配前面**0**个或**n个**字符，因此要越过" * "，匹配规则至少退到上一步，即比较p[j-1]和s[i]
        + **p[j-1] != s[i]**
          + **dp[i][j] = dp[i][j-2]**
            + 对应于匹配前一个字符**0**次
            + 例如：s = 'aa'，p = 'aac*'，那么此时，就要将匹配规则后退2步，即用'aa'去匹配'aa'
        + **p[j-1] == s[i]**
          + **dp[i][j] = dp[i][j-2]**
            + 对应于匹配前一个字符**0**次，或者说不匹配
            + 例如：s = 'ab'，p = 'abb*'，那么此时，就要将匹配规则后退2步，即用'ab'去匹配'ab' 
          + **dp[i][j] = dp[i][j-1]**
            + 对应于匹配前一个字符**1**次
            + 例如：s = 'ac'，p = 'ac*'，那么此时，就要将匹配规则后退1步，即用'ac'去匹配'ac'
          + **dp[i][j] = dp[i-1][j]**
            + 对应于匹配前一个字符**n**次
            + 例如：s = 'abbbb'，p = 'ab*'，
              + 那么此时，就要将匹配规则前进n-1步，即复制n-1份前一个字符b
              + 相当于s后退n-1步
                + 又因为动态规划属于递推，所以每次s后退一步就可以了
        + **p[j] == " . "**
          + 同**p[j-1] == s[i]**，所以将此条件归并但上一步的条件中即可
  + 初始化
    + s[i]的前0个字符 和 p[j]的前j个字符能否匹配
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
    for(let i = 0;i <= m;i++){
        if(p[i] == '*' && dp[0][i-1]){
            dp[0][i+1] = true;
        }
    }
    for(let i = 1;i <= n;i++){
        for(let j = 1;j <= m;j++){
            if(p[j-1] == s[i-1] || p[j-1] == '.'){
                dp[i][j] = dp[i-1][j-1]
            }else if(p[j-1] == '*'){
                if(p[j-2] != s[i-1]){
                    dp[i][j] = dp[i][j-2];
                }
                if(p[j-2] == s[i-1] || p[j-2] == '.'){
                    dp[i][j] = dp[i-1][j] || dp[i][j-1] || dp[i][j-2];
                }
            }
        }
    }
    return dp[n][m];
};
```