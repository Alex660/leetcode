# 转换成小写字母 （简单）
# 题目描述
![截屏2019-12-05上午7.26.52.png](https://pic.leetcode-cn.com/2981eacde0f821ef0fd751d99bea3f73c92933a84487d874bec22c8d6f0b9b5b-%E6%88%AA%E5%B1%8F2019-12-05%E4%B8%8A%E5%8D%887.26.52.png)
# 题目地址
<https://leetcode-cn.com/problems/to-lower-case/submissions/>
#### 解法一：暴力枚举 + ASCII码转换
+ [a,z] = [97,122]
+ [A,Z] = [65,90]
```javascript
/**
 * @param {string} str
 * @return {string}
 */
var toLowerCase = function(str) {
    let result = '';
    for(let i = 0;i < str.length;i++){
        let code = str.charCodeAt(i);
        if(code <= 90 && code >= 65){
            result += String.fromCharCode(code + 32);
        }else{
            result += str[i];
        }
    }
    return result;
};
```
#### 解法二：位运算
+ 大写变小写、小写变大写 : ASCII码 ^= 32
+ 大写变小写、小写变小写 : ASCII码 |= 32
+ 小写变大写、大写变大写 : ASCII码 &= -33
```javascript
/**
 * @param {string} str
 * @return {string}
 */
var toLowerCase = function(str) {
    let result = '';
    for(let i = 0;i < str.length;i++){
        result += String.fromCharCode(str.charCodeAt(i) | 32);
    }
    return result;
};
```
#### 解法三：库函数
```javascript
/**
 * @param {string} str
 * @return {string}
 */
var toLowerCase = function(str) {
    return str.toLowerCase();
};
```