# 颠倒二进制位（简单）
# 题目描述
![截屏2019-11-25下午11.20.14.png](https://pic.leetcode-cn.com/539e486f2d5626100b38fd89a8f92d4be83bde5e7687db03c7814f4a7d66f461-%E6%88%AA%E5%B1%8F2019-11-25%E4%B8%8B%E5%8D%8811.20.14.png)
![截屏2019-11-25下午11.20.23.png](https://pic.leetcode-cn.com/21d4b686255b26992f94429366bc096865d5216e54234f61355e4d9b7f5e553b-%E6%88%AA%E5%B1%8F2019-11-25%E4%B8%8B%E5%8D%8811.20.23.png)
# 题目地址
<https://leetcode-cn.com/problems/reverse-bits/>
#### 解法一：位移 + 拼接
+ “>>”运算符执行有符号右移位运算。与左移运算操作相反，它把 32 位数字中的所有有效位整体右移，再使用符号位的值填充空位。移动过程中超出的值将被丢弃。
  + ![截屏2019-11-26上午5.56.53.png](https://pic.leetcode-cn.com/d407ff5e6a7f4324ad1a903b0c8dcadb177caba2c44c7b103378bc7bf4a3088d-%E6%88%AA%E5%B1%8F2019-11-26%E4%B8%8A%E5%8D%885.56.53.png)
+ “<<”运算符执行左移位运算。在移位运算过程中，符号位始终保持不变。如果右侧空出位置，则自动填充为 0；超出 32 位的值，则自动丢弃。
  + ![截屏2019-11-26上午5.57.09.png](https://pic.leetcode-cn.com/d0a7d2fd7fc6141b9d854ed8793e5809e7d03e828f8b304662aec1f98713c93a-%E6%88%AA%E5%B1%8F2019-11-26%E4%B8%8A%E5%8D%885.57.09.png)
  + ![截屏2019-11-26上午5.57.55.png](https://pic.leetcode-cn.com/97d30e75b23bc649d4a7e1f1742cb1f6e4236d37bb0800fdfe15c294aa822608-%E6%88%AA%E5%B1%8F2019-11-26%E4%B8%8A%E5%8D%885.57.55.png)
+ “>>>”运算符执行五符号右移位运算。它把无符号的 32 位整数所有数位整体右移。对于无符号数或正数右移运算，无符号右移与有符号右移运算的结果是相同的。
  + 对于负数，左侧空位不再用符号位的值来填充，而是用 0 来填充。
  + ![截屏2019-11-26上午5.58.25.png](https://pic.leetcode-cn.com/a08279208b7d92d94be8fc5aa78e1f4a3258ef14c168cf3627095b9284782947-%E6%88%AA%E5%B1%8F2019-11-26%E4%B8%8A%E5%8D%885.58.25.png)
+ 每次获取原数的最低位，拼接到结果数字结尾里
  + 获得下一位时右移原数一位
  + 拼接下一位时提前左移结果数一位
```javascript
/**
 * @param {number} n - a positive integer
 * @return {number} - a positive integer
 */
var reverseBits = function(n) {
    let result = 0;
    for(let i = 0;i < 32;i++){
        result = (result << 1) + (n & 1);
        n >>= 1;
    }
    return result >>> 0;
};
```
#### 解法二：位移 + 换位
```javascript
/**
 * @param {number} n - a positive integer
 * @return {number} - a positive integer
 */
var reverseBits = function(n) {
    let result = 0;
    // result从右往移动空出末位 + n从左往右移动获取末位 + n次 = 倒序
    for(let i = 0;i < 32;i++){
        // 左移空出一位
        result <<= 1
        // n&1获取n的末位，result的末位换成n的末位
        result |= n & 1;
        // 右移1位
        n >>= 1;
    }
    return result >>> 0;
};
```
#### js中表达式 >>> 0 浅析
+ ">>>" 是无符号右移
  + 负数移动n位，总是非负数
  + ">>> 0"意义
    + 将数据转换为number类型
    + 将number转换为无符号的32bit数据
      + 即Uint32类型
        + 如不能转换为Number，则为0
        + 如果为非整数，先转换为整数
          + parseInt(string,radix)
          + Math.trunc()
          + ~~n
          + n | n
          + n | 0
          + n << 0
          + n >> 0
          + n & n
+ ">>" 是有符号右移
  + 负数移位是负数