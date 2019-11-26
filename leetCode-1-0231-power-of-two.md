# 2的幂（简单）
# 题目描述
![截屏2019-11-25下午10.27.16.png](https://pic.leetcode-cn.com/435c2afdcdda84e032e757135db35a3f0f3402a207f8b603156dee3099e8e696-%E6%88%AA%E5%B1%8F2019-11-25%E4%B8%8B%E5%8D%8810.27.16.png)
# 题目地址
<https://leetcode-cn.com/problems/power-of-two/>
#### 解法一：2的幂数的数字的二进制特点 + 位操作
+ 2的幂数的数字的二进制有且只有一个1，其余均是0
+ n & (n-1)：清零最低位的1
  + 合起来 n & (n-1) == 0
```javascript
/**
 * @param {number} n
 * @return {boolean}
 */
var isPowerOfTwo = function(n) {
    return n > 0 && (n & (n-1)) == 0;
};
```
#### 解法二：调用函数懒蛋法
```javascript
/**
 * @param {number} n
 * @return {boolean}
 */
var isPowerOfTwo = function(n) {
  return Number.isInteger(Math.log2(n));
};
```
#### 解法三：位运算
```javascript
/**
 * @param {number} n
 * @return {boolean}
 */
var isPowerOfTwo = function(n) {
  return n > 0 && (n & (-n)) == n
};
```
#### 解法四：取模
```javascript
/**
 * @param {number} n
 * @return {boolean}
 */
var isPowerOfTwo = function(n) {
  while(n > 1){
      n /= 2;
  }
  if(n == 1){
      return true;
  }else{
      return false;
  }
};
```