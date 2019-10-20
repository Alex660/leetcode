# 字符串转换整数（atoi）
# 题目地址
<https://leetcode-cn.com/problems/string-to-integer-atoi/solution/>
#### 解法：正则

+ 去首尾控格
+ 正则
    + ([])  一个中括号表达式
    + (?)   匹配前面字表达式零次或一次，或指明一个非贪婪限定符
    + (\d)  匹配一个数字字符 等价于 <=> [0-9]
    + (+)   匹配前面的子表达式一次或多次 等价于 <=> {1,}
```javascript
/**
 * @param {string} str
 * @return {number}
 */
var myAtoi = function(str) {
    if(!str){
        return 0;
    }
    str = str.trim();
    var reg = new RegExp(/^[+|-]?\d+/g);
    if(!reg.test(str)){
        return 0;
    }
    var num = str.match(reg)[0];
    var max = Math.pow(2,31);
    if(num>max-1){
        return max-1;
    }else if(num < -max){
        return -max;
    }
    return num;
};
```