# Pow(x,n)（中等）
# 题目描述
![截屏2019-10-25上午10.10.29.png](https://pic.leetcode-cn.com/9388e716b89623fe1a2e702b6ea53a4b31756a23c7073532ba7f710e48029ccf-%E6%88%AA%E5%B1%8F2019-10-25%E4%B8%8A%E5%8D%8810.10.29.png)
![截屏2019-10-25上午10.10.46.png](https://pic.leetcode-cn.com/c11fc5ba57fe0d709425aa1042a1d28bc8178956ab0876f5f822dc9cda37f16d-%E6%88%AA%E5%B1%8F2019-10-25%E4%B8%8A%E5%8D%8810.10.46.png)
# 题目地址
<https://leetcode-cn.com/problems/powx-n/solution/powx-n-by-leetcode/>
#### 解法一：暴力加乘
+ 时间复杂度 O(n) 原数自身连乘n次
+ 空间复杂度 O(1)
+ 注意：有可能超出时间限制 js数字只有Number类型 双精度浮点数存储 在2的-53次方到2的53次方之间
  + 新增的BigInt 内置对象可以表示任意大的数
```javascript
/**
 * @param {number} x
 * @param {number} n
 * @return {number}
 */
var myPow = function(x, n) {
    if( n == 0){
        return 1;
    }
    x = parseFloat(x);
    if(n < 0){
        x = parseFloat(1/x);
        n = -n;
    }
    var tmp = x;
    while(n > 1){
        x *= tmp;
        n--;
    }
    return x; 
};
```
#### 解法二：分治
+ 时间复杂度：O(logn)
  + 每次计算自己的一半
+ 空间复杂度：O(logn)
  + 因为每次递归要存储 x^(n/2) 的结果 计算O(logn)次
+ 技巧递归
  1. terminator
  2. process（split your big problem
  3. drill down (subproblmes)
  4. merge(subresult)
  5. reverse states
  > x^n --> 2^10 == (2^5)*(2^5)  
    2^5 == (2^2)*(2^2)*2  
    2^2 == (2^1)*(2^1)  
    -->  
    pow(x,n):  
    subproblem:subresult = pow(x,n/2)  
    merge:  
    if( n%2 == 1){
        //odd  
        result = subresult * subresult * x  
    }else{  
        //even   
        result = subresult*subresult  
    }
```javascript
/**
 * @param {number} x
 * @param {number} n
 * @return {number}
 */
function divide(x,n){
    if( n == 0){
        return 1;
    }
    var subresult = divide(x,parseInt(n/2));
    if(n&1 == 1){
        return subresult * subresult *x;
    }else{
        return subresult * subresult;
    }
}
var myPow = function(x, n) {
    x = parseFloat(x);
    if(n < 0){
        x = parseFloat(1/x);
        n = -n;
    }
    return divide(x,n); 
};
```
#### 解法三：迭代法 解法二的优化版
+ 据说是牛顿迭代法 有兴趣的可以去查查哈
+ 时间复杂度：O(logn) 本质跟解法二一样
+ 空间复杂度：只需要两个额外变量，所以是O(1)
+ 由解法二核心函数divide可知
  1. 每次取一半相当于循环计算了n/2次
  2. 奇数和偶数n的结果差异只是多乘了一个自己而已 
  3. 1步可知每次n/2 无论是偶数n还是奇数n最后除2取整都为1
      + 但奇数n不除2之前先取膜但话就为1
      + 如 n = 5 -->  5,2,1
      + 如 n = 4 -->  4,2,1   
  4. 所以迭代中可以设置循环体内第一次就取余，为奇数则乘以它自己 
      + 不论奇数还是偶数n，循环体内每次都要乘以自身
  5. 由4可知，如果是奇数那第一次就会多乘一个自己，不是则每次乘以x即n个x相乘的结果
```javascript
/**
 * @param {number} x
 * @param {number} n
 * @return {number}
 */
var myPow = function(x, n) {
    if( n == 0){
        return 1;
    }
    x = parseFloat(x);
    if(n < 0){
        x = parseFloat(1/x);
        n = -n;
    }
    var subresult = x;
    var result = 1;
    for(var i = n;i>0;i=parseInt(i/2)){
        if(i&1==1){
            result = result*subresult;
        }
        subresult = subresult * subresult;
    }
    return result;
};
```