# 回文数（简单）
# 题目地址
<https://leetcode-cn.com/problems/palindrome-number/solution/>
#### 解法一：字符串反转法
```javascript
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function(x) {
    if(x<0){
        return false;
    }else if(String.prototype.split.call(x,'').reverse().join('')!=x){
        return false;
    }
    return true;
}
```
#### 解法二：中心扩展法
```javascript
var isPalindrome = function(x) {
    var middle = parseInt(x.length/2)
    var left = middle-1,right = middle+1;
    while(left>=0 && right<x.length){
        if(x[left] != x[right]){
        return false;
        }
        left--;
        right++;
    }
    return true;
}
```
#### 解法三：二分对比法 
+ 将原整数分成前后两部分
+ 从最后一位数字开始取出直到一半
+ 原数字不断/10抛弃最后一位，新数字不断乘以10 直到反转后的数大于原数 即可判断到了一半
+ 奇数数字/10 == 反转数字  || 偶数数字 == 反装数字  => 是回文数
```javascript
var isPalindrome = function(x) {
    if(x<0 || (x%10 == 0 && x!=0)){
        return false;
    }
    var reverseNumber = 0;
    while(x>reverseNumber){
        reverseNumber = reverseNumber*10 + x%10;
        x = parseInt(x/10);
    }
    return x == reverseNumber || x == parseInt(reverseNumber/10)
};
```
