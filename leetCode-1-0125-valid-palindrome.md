# 验证回文串（简单）
# 题目描述
![截屏2019-12-06下午2.42.22.png](https://pic.leetcode-cn.com/0db0e3ec95a953097c77087161bb32b3ac61ea808d57d53c4ba87c1cbbf8d9ac-%E6%88%AA%E5%B1%8F2019-12-06%E4%B8%8B%E5%8D%882.42.22.png)
# 题目地址
<https://leetcode-cn.com/problems/valid-palindrome/>
#### 解法一：调用函数懒蛋法
```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var isPalindrome = function(s) {
    let strArr = s.replace(/[^0-9a-zA-Z]/g,"").toLowerCase().split('');
    return strArr.join('') == strArr.reverse().join('');
};
```
#### 解法二：格式化 + 双指针夹逼
```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var isPalindrome = function(s) {
    s = s.replace(/[^0-9a-zA-Z]/g,'').toLowerCase();
    let n = s.length;
    let left = 0;
    let right = n-1;
    while(left < right){
        if(s[left] != s[right]){
            return false;
        }
        left++;
        right--;
    }
    return true;
};
```