# 有效的括号（简单）
# 题目描述
![截屏2019-11-10上午5.31.16.png](https://pic.leetcode-cn.com/0f3cedad553008800a1911bc2f4274048bfdca4b84f94af0f8dc842d5c4ed4a8-%E6%88%AA%E5%B1%8F2019-11-10%E4%B8%8A%E5%8D%885.31.16.png)
![截屏2019-11-10上午5.31.27.png](https://pic.leetcode-cn.com/60095ccbf94027ec690210aec2d468acbe6f854de45cb0e87b00f8527376f043-%E6%88%AA%E5%B1%8F2019-11-10%E4%B8%8A%E5%8D%885.31.27.png)
# 题目地址
<https://leetcode-cn.com/problems/valid-parentheses/>
#### 解法：洋葱栈
+ 由题意可知此题是判断输入的字符串是否有效
  + 即 **n个洋葱** 结构
    + 形如()、(())、()((())) 【左开右闭】
      + 即遇到的第一个右闭字符其左边一定能找到一个左开字符与其一一对应合成**一层洋葱结构**
      + 如果将找到的一组字符去掉，那么下次寻找操作和上一步一摸一样 
        + 找到后去掉并循往复
          + 如果找到的最近的左边字符不是与其一一对应的左开字符，则说明**整体洋葱结构**受到了破坏，不是有效的字符串
          + 如果找到了，继续遍历
            + 直到循环结束后，没有剩余的**左半洋葱皮**就说明是有效的字符串 
      + 总体合并起来若符合题意即为**n层洋葱结构**
      + **具象化**上述遇到右闭字符而去寻找左开字符且找到后去掉并循往复查找的动作
        + 想象一下有哪一种数据结构，可以做到
          + 遍历**洋葱**即原字符串
            + 遇到左开字符时，加入进去
            + 遇到右闭字符时
              + 左边是空的
                + 则**洋葱结构**受到破坏，返回false 
              + 左边不是空的，为左开字符
                + 如果这个左开字符与当前右闭字符一一对应，则找到了符合题意的**一层洋葱结构**；且去掉当前左字符，继续遍历下一个遇到的右闭字符
                + 否则，因**局部洋葱结构**受到了破坏，因而**整体洋葱结构**破坏，是一个无效的洋葱，即无效字符串 
          + 栈能实现
            + 找最近的左开字符
              + 对应先入后出
            + 去掉最近的左开字符  
              + 对应出栈
    + 而题中只不过是把"()"换成了"()"、"[]"和"{}"而已
      + 因此只需再增加一层hashMap对应法,即可
        + "(" ---> ")"  
        + "[" ---> "]"  
        + "{" ---> "}"  
```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
    var stack = [];
    var map = new Map();
    map.set("(",")");
    map.set("{","}");
    map.set("[","]");
    for(var i = 0;i < s.length;i++){
        if(!map.get(s[i])){
            if(stack.length == 0){
                return false;
            }
            var topEle = stack.pop();
            if(map.get(topEle) != s[i]){
                return false;
            }
        }else{
            stack.push(s[i]);
        }
    }
    return stack.length == 0;
};
```
+ 上述遇到左边为空的情况时，可以合并到判断 右闭字符 是否对应 左开字符的情况中去
  + 即给栈增加初始值：‘#’ ！= 右闭字符
    + 则循环会在中途直接退出
+ 若没有遇到
  + 则循环会遍历完
    + 左开字符组是否被匹配完全部出栈
      + 是，则true
      + 否，false     
```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
    var stack = [];
    var map = new Map();
    map.set("(",")");
    map.set("{","}");
    map.set("[","]");
    for(var i = 0;i < s.length;i++){
        // 闭括号
        if(!map.get(s[i])){
            var topEle = stack.length == 0 ? '#' : stack.pop();
            if(map.get(topEle) != s[i]){
                return false;
            }
        }
        // 开括号
        else{
            stack.push(s[i]);
        }
    }
    return stack.length == 0;
};
```
