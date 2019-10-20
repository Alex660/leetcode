# 字符串转换整数（atoi）（中等）
# 题目描述
![截屏2019-10-20上午9.27.24.png](https://pic.leetcode-cn.com/d7249868e23a50f913b6ee42cb035912fd79ce6d18b6b68df908420163b6519b-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.27.24.png)
![截屏2019-10-20上午9.27.36.png](https://pic.leetcode-cn.com/9b1601eceb92f20772d91d9532ecc05175520877fe0793665604cbb09512a569-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.27.36.png)
![截屏2019-10-20上午9.27.43.png](https://pic.leetcode-cn.com/cd279f82f086b6e972af8d5bc7ec8fef0bd9954ed3f2f774f8f78e7581ecc852-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.27.43.png)
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