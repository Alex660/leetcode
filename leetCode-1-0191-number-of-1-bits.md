# 位1的个数（简单）
# 题目描述
![截屏2019-11-25下午4.52.25.png](https://pic.leetcode-cn.com/8e4a6e74e5e89f8052bad31854b482596231b9901b4b693e45720f53a14794d3-%E6%88%AA%E5%B1%8F2019-11-25%E4%B8%8B%E5%8D%884.52.25.png)
![截屏2019-11-25下午4.52.37.png](https://pic.leetcode-cn.com/70524435d3326fdf44ea6dec63dec185163e9b8e63e137c21efc06f24351b464-%E6%88%AA%E5%B1%8F2019-11-25%E4%B8%8B%E5%8D%884.52.37.png)
# 题目地址
<https://leetcode-cn.com/problems/number-of-1-bits/>
#### 解法一：循环和位移动
+ 时间复杂度：O（1）
+ 空间复杂度：O（1）
+ 任何数字跟掩码1进行逻辑与运算，都可以获得这个数字的最低位
+ 检查下一位时，将掩码左移一位
  + 0000 0000 0000 0000 0000 0000 0000 0001 =>
  + 0000 0000 0000 0000 0000 0000 0000 0010
```javascript
/**
 * @param {number} n - a positive integer
 * @return {number}
 */
var hammingWeight = function(n) {
    let count = 0;
    let mask = 1;
    for(let i = 0;i < 32;i++){
        if((n & mask) != 0){
            count++;
        }
        mask <<= 1;
    }
    return count;
};
```
#### 解法二：位操作技巧
+ 时间复杂度：O(1)(最坏情况下n中所有位均为1)
+ 空间复杂度：O(1)
+ 每次把数字最后一个二进制位1反转为0，sum++
  + 当没有1可反的时候，数字变成了0
+ n & (n-1) 
  + 清零最低位的1
+ 借用官方一张图说明
  + ![截屏2019-11-25下午5.40.55.png](https://pic.leetcode-cn.com/19586b694a2c3dda12485ac667c7b065827f223851dc6c71c6f8fb23988e6114-%E6%88%AA%E5%B1%8F2019-11-25%E4%B8%8B%E5%8D%885.40.55.png) 
  + n数字的二进制的最低位的1总是对应n-1数字的二进制的0
    + 相与后，其它位不变，当前位变成0
```javascript
/**
 * @param {number} n - a positive integer
 * @return {number}
 */
var hammingWeight = function(n) {
    let sum = 0
    while(n != 0){
        sum++
        n &= (n-1)
    }
    return sum
};
```
#### 解法三：调用函数懒蛋法
+ toString(2)转二进制
+ 正则匹配二进制某个数出现的次数
```javascript
/**
 * @param {number} n - a positive integer
 * @return {number}
 */
var hammingWeight = function(n) {
    return ((n.toString(2).match(/1/g)) ||[]).length;
};
```
#### 解法四：模拟十转二进制、取模
+ [如何从十进制转换为二进制](https://zh.wikihow.com/%E4%BB%8E%E5%8D%81%E8%BF%9B%E5%88%B6%E8%BD%AC%E6%8D%A2%E4%B8%BA%E4%BA%8C%E8%BF%9B%E5%88%B6)
+ ">>>"为无符号左边填充0，">>"为有符号填充 
```javascript
/**
 * @param {number} n - a positive integer
 * @return {number}
 */
var hammingWeight = function(n) {
    let count = 0;
    while(n){
        // n % 2 == 1
        if(n & 1 == 1){
            count++;
        }
        n >>>= 1;
    }
    return count;
};
```