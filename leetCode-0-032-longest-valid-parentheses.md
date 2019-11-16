# 最长有效括号（困难）
# 题目描述
![截屏2019-11-15下午11.06.12.png](https://pic.leetcode-cn.com/afbb9a926c3c1858275fa5ce0eb02a84c88222385b3829202ab1ef377370dd53-%E6%88%AA%E5%B1%8F2019-11-15%E4%B8%8B%E5%8D%8811.06.12.png)
# 题目地址
<https://leetcode-cn.com/problems/longest-valid-parentheses/>
#### 解法一：栈 + 暴力
+ 时间复杂度：O(n^2)
+ 空间复杂度：O(n)
+ 类似题型
  + [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/solution/20-you-xiao-de-gua-hao-by-alexer-660/)
  + 由题意可知判断有效字符串
    + 有效，在偶数长度字符串内产生
    + 是否，用栈解决，完全同20题
      + 长度则可以通过有效子串的前后索引求差可得
+ 因暴力枚举，所以超时
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var longestValidParentheses = function(s) {
    var max = 0;
    function isValid(str){
        var stack = [];
        for(var i = 0;i < str.length;i++){
            if(str[i] == '('){
                stack.push('(');
            }else{
                if(stack.length == 0){
                    return false;
                }
                var topEle = stack.pop();
                if(topEle != '('){
                    return false;
                }
            }
        }
        return stack.length == 0;
    }
    for(var i = 0;i < s.length;i++){
        for(var j = i + 2;j <= s.length;j+=2){
            if(isValid(s.slice(i,j))){
                max = Math.max(max,j-i);
            }
        }
    }
    return max;
};
```
+ 当然也可以给stack设置**哨兵**，else判断里就不用每次判断stack是否empty了
  + 虽然也是超时，但是解法二会用到这个小技巧
    ```javascript
    /**
     * @param {string} s
    * @return {number}
    */
    var longestValidParentheses = function(s) {
        var max = 0;
        function isValid(str){
            var stack = ['#'];
            for(var i = 0;i < str.length;i++){
                if(str[i] == '('){
                    stack.push('(');
                }else{
                    var topEle = stack.pop();
                    if(topEle != '('){
                        return false;
                    }
                }
            }
            return stack.length == 1;
        }
        for(var i = 0;i < s.length;i++){
            for(var j = i + 2;j <= s.length;j+=2){
                if(isValid(s.slice(i,j))){
                    max = Math.max(max,j-i);
                }
            }
        }
        return max;
    };
    ```
#### 解法二：栈 + 取最优解 + 哨兵
+ 时间复杂度：O(n)
+ 空间复杂度：O(n)
+ 思路
  + 栈和取最优解
    + 参考解法一可知
      + 与其通过遍历所有子串的组合再寻找其中的最优解
      + 不如在遍历过程中就去判断前i个字符的有效性并取最优解
  + 哨兵
    + 第一个元素就是右括号时，栈内还没有元素，违背是左括号才入栈的原则
    + 当栈为空时，将右括号入栈，当做哨兵节点继续遍历
  + 图解
    ![截屏2019-11-16下午10.36.07.png](https://pic.leetcode-cn.com/185cddf827143b36940095a768047e4cedcbce569ad3cba7ea8c5227816046c3-%E6%88%AA%E5%B1%8F2019-11-16%E4%B8%8B%E5%8D%8810.36.07.png)
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var longestValidParentheses = function(s) {
    var max = 0;
    var stack = [-1];
    for(var i = 0;i < s.length;i++){
        if(s[i] == '('){
            stack.push(i);
        }else{
            stack.pop();
            if(stack.length == 0){
                stack.push(i);
            }else{
                max = Math.max(max,i - stack[stack.length-1]);
            }
        }
    }
    return max;
};
```
#### 解法三：两边夹逼 + 直观法
+ 时间复杂度：O(n)
+ 空间复杂度：O(1)
  + 一个有效的括号字符串形如：'()'、'(())'、'()()'，无非就是这三种
  + 反过来形如：'(()'、'())'、'('、')'整体上就是无效的
  + 设置left、right两个变量
    + 从左到右遍历  
      + 遇到'('，left++
      + 遇到')'，right++
      + 当left == right 时，就属于有效括号组合内
      + 当right > left 时，就属于无效括号组合内
        + 如'())'
        + 并且初始化left = right = 0，继续向右遍历
    + 以上循环后，重置left = right = 0
    + 同理继续从右到左遍历
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var longestValidParentheses = function(s) {
    var max = 0;
    var left = 0;
    var right = 0;
    for(var i = 0;i < s.length;i++){
        if(s[i] == '('){
            left++;
        }else{
            right++;
        }
        if( left == right){
            max = Math.max(max,2 * left);
        }else if(right > left){
            left = right = 0;
        }
    }
    left = right = 0;
    for(var i = s.length-1;i >= 0;i--){
        if(s[i] == '('){
            left++;
        }else{
            right++;
        }
        if(left == right){
            max = Math.max(max,right * 2);
        }else if(left > right){
            left = right = 0;
        }
    }
    return max;
};
```
#### 解法四：动态规划
+ 时间复杂度：O(n)
+ 空间复杂度：O(n)
+ 图解
    + ![截屏2019-11-16下午10.36.22.png](https://pic.leetcode-cn.com/d7a25d27a2d90561d5cf6f780b1cdbff5c079cc112c8d28d94aebe31ce41aab6-%E6%88%AA%E5%B1%8F2019-11-16%E4%B8%8B%E5%8D%8810.36.22.png) 
+ 由解法三和题意可知
  + 一个有效字符串的结尾字符一定是")"
  + 在此我们设状态转移数组**dp[i]**
    + 表示以下标**i**的字符为结尾的最长有效子字符串的长度
  + 而以")"字符为结尾的有效的字符串无非是以下几种形式
    + 1、"**(**" + "**)**" <=> **...()**
      + 等价于：s[i] == ')' && s[i-1] == '('
      + 因此最后两个字符一定是最长有效子字符串的一部分，长度为2
      + 递推公式为：
        + **dp[i] = dp[i-2] + 2**
    + 2、"**)**" + "**)**" <=> **...))**
      + 2.1、"**)**" + "**)**" + "**)**" <=> **...)))**
        + 等价于：s[i] == ')' && s[i-1] == ')' && s[i-2] == ')'
        + 递推公式为：
        + **dp[i] = dp[i-1] + 0**
      + 2.2、"**(**" + "**)**" + "**)**" <=> **...())**
        + 等价于：s[i] == ')' && s[i-1] == ')' && s[i-2] == '('
        + 此处：s[i-2] == '('
          + 说明s[i-2] 和 s[i-1]可以构成一对有效括号
          + 等价于以i-1为结尾的dp[i-1]最长有效子字符串的长度的最后一部分子子字符串"()"
          + 此种情况下：s[i-2] == dp[i-dp[i-1]-1] == '('
        + 反过来想，就是当前一个合法序列的前边一个位置刚好是左括号时
        + 即当dp[i-dp[i-1]-1] == '('时，
          + dp[i-dp[i-1] - 2] + 2
          + **意即最后一个括号")"是和前一个合法序列的前一个位置"("组成一对，加2**
          + 如图：**2+2 = 4**处
        + 递推公式为：
          + **dp[i] = dp[i-1] + dp[i-dp[i-1]-2]+2**
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var longestValidParentheses = function(s) {
    var max = 0;
    var n = s.length;
    var dp = new Array(n).fill(0);
    for(var i = 1;i < n;i++){
       if(s[i] == ')'){
           // 右括号前边是左括号
           if(s[i-1] == '('){
               dp[i] = ( i >= 2 ? dp[i-2] : 0) + 2;
           }
           // 当前右括号前边是右括号，并且前一个合法子序列的前边是左括号和当前右括号组成一对，则最长子序列个数加2
           else if(i - dp[i-1] > 0 && s[i - dp[i-1] - 1] == '('){
               dp[i] = dp[i-1] + ( (i - dp[i-1] >= 2) ? dp[i - dp[i-1] - 2] : 0 ) + 2;
           }
            max = Math.max(max,dp[i]);
       }
    }
    return max;
};
```