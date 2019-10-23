# 括号生成（中等）
# 题目描述
![截屏2019-10-23上午11.26.30.png](https://pic.leetcode-cn.com/2e1003273b9127a4c7320a4ef7fd308b2ea99849c4392de8e43041e4334eec6c-%E6%88%AA%E5%B1%8F2019-10-23%E4%B8%8A%E5%8D%8811.26.30.png)
# 题目地址
<https://leetcode-cn.com/problems/generate-parentheses/>
#### 解法一：递归 DFS
+ 递归模版
    ```java
    //  recur 递归
    public void recur(int level, int param) { 
        // terminator 终结条件
        if (level > MAX_LEVEL) { 
        // process result 流程结果
        return; 
        } 
        // process current logic  当前逻辑过程
        process(level, param); 
        // drill down 向下递归
        recur( level: level + 1, newParam); 
        // restore current status 恢复现状
    }
    ```  
+ 由此可分析出所有递归情况代码如下
```javascript
/**
 * @param {number} n
 * @return {string[]}
 */
var generateParenthesis = function(n) {
    _generate(0,2*n,'');
    return null;
};
function _generate(level,max,str){
    // terminator
    if(level >= max){
        console.log(str);
        return;
    }
    // process
    // var s1 = s +'(';
    // var s2 = s + ')';
    // drill down
    _generate(level+1,max,str+'(');
    _generate(level+1,max,str+')');
    // reverse states
    // 没有全局变量所以忽略局部变量它自动会最后清除
}
```
+ 但由题意必须符合条件去除不合法的情况
+ filter the invalid str
  1. left 随时加，不能超n
  2. right 必须之前有左括号 && 左括号个数 > 右括号个数+ 
+ 所以进一步改写代码如下
```javascript
/**
 * @param {number} n
 * @return {string[]}
 */
var generateParenthesis = function(n) {
    var result = [];
    function _generate(left,right,n,str){
        // terminator
        if(left == n && right == n){
            // filter the invalid str
            //1、left 随时加，不能超n
            //2、right 必须之前有左括号 && 左括号个数 > 右括号个数
            console.log(str);
            result.push(str)
            return;
        }
        // process
        // var s1 = s +'(';
        // var s2 = s + ')';
        // drill down
        if(left < n){
            // 加一个左括号
            _generate(left+1,right,n,str+'(');
        }
        if(left > right){
            // 加一个右括号
            // 此处 right 一定小于 n 因为 left 小于 n
            _generate(left,right+1,n,str+')');
        }
        // reverse states
        // 没有全局变量所以忽略局部变量它自动会最后清除
    }
    _generate(0,0,n,'');
    return result;
};
``` 
#### 解法二：动态规划
> n = 3  
> [  
    "((()))",  
    "(()())",  
    "(())()",  
    "()(())",  
    "()()()"  
]
+ 观察示例规律
+ 分步 n
+ 要解：
  + 第 n 步 要像前几步转换 如 f3 = f1/f2/f1,f2 ------------------------ ***注意***
   + f(0)：""
   + f(1)："()" == "("+f(0)+")" == "("f(0)")" --------------------------- ***注意***
   + f(2)：
     + "()()" == "("+f(0)+")" + "(" + ")" == "("+f(0)+")" + f(1)
       + == "(f(0)")"f(1) -------------------------------------- ***注意***
     + "(())"
       + == "(" + f(1) +")"
       + == "("f(1)")" ---------------------------------------- ***注意***
   + f(3)：
      + "((()))" && f(2) == "(())" 时 
        + == "(" + f(2) + ")" == "("f(2)")" ---------------------- ***注意***
      + "(()())" && f(2) == "()()" 时
        + == "(" + f(2) + ")" == "("f(2)")" ---------------------- ***注意***
      + "(())()" && f(2) == "(())" 时
        + == f(2) + "(" + ")"
        + == f(2)"("f(0)") ------------------------------------- ***注意***
      + "()(())" 同理如上
        + == "("f(0)")f(2)
      + "()()()" && f(2) == "()()" 时
        + == f(1) + f(2)
        + == f(1) + "(" + "f(1)" +")"
        + == f(1)"("f(1))" ------------------------------------------ ***注意***
+ 要解合并
  + f(0) == ""
  + f(1) == (f(0))
  + f(2) == (f(0))f(1)   + (f(1))f(0)
  + f(3) == (f(0))f(2)   + (f(1))f(1)   + (f(2))f(0)
  + ...
  + f(n) == ((f0))f(n-1) + (f(1))f(n-2) + (f(2))f(n-3) + ... +(f(i))f(n-i-1) + ... + (f(n-1))

+ 根据上面的f(n)公式写出代码如下
```javascript
/**
 * @param {number} n
 * @return {string[]}
 */
var generateParenthesis = function(n) {
    var results = [[""]];
    for(var i = 1;i<=n;i++){
        var result = [];
        for(var j = 0;j < i;j++){
            var first = results[j];
            var second = results[i-j-1]
            for(var key in first){
                var tmpFirst = first[key];
                for(var secondKey in second){
                    var tmpSecond = second[secondKey];
                    result.push( "("+ tmpFirst +")" + tmpSecond);
                }
            }
        }
        results.push(result);
    }
    return results[results.length-1];
};
```
+ 进一步分析
  + f(0) == ""  
  + f(1) == (f(0)) == "()"  
  + f(2) == (f(0))f(1)   + (f(1))f(0) == ()f(1) + (f(1)) 
  + f(3) == (f(0))f(2)   + (f(1))f(1)   + (f(2))f(0) 
    + == ()f(2) + (f(1))f(1) + (f(2))
  + ...  
  + f(n) == ((f0))f(n-1) + (f(1))f(n-2) + (f(2))f(n-3) + ... +(f(i))f(n-i-1) + ... + (f(n-1))
    + ==  "(" + f(n-1)的一部分排列 +")" + f(n-1)的另一部分排列
    + ==  "(" + X +")" + Y
    + X 从0到n Y从到n 
      + 0 代表 f(n-1)的所有情况在 另一边
      + 大于0 && 小于n 代表 f(n-1)的部分情况在最后一组括号内部 另一部分在外边
  + 最终分析得出核心思想
    + 当 i (遍历索引)等于 n 时，所求为最后一组括号与前面 n-1 种情况的排列组合情况 细分即为 要解合并处。