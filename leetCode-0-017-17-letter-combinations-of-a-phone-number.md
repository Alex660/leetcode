# 电话号码的字母组合（中等）
# 题目描述
![截屏2019-10-26下午7.36.33.png](https://pic.leetcode-cn.com/521badfb2141a238e6f7e4328c7523086161ee5cb98da3cd2121c7011667447f-%E6%88%AA%E5%B1%8F2019-10-26%E4%B8%8B%E5%8D%887.36.33.png)
# 题目地址
<https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/>
#### 解法一：递归
+ 回溯
 ![截屏2019-10-26下午10.35.32.png](https://pic.leetcode-cn.com/3ef7735f2a30e9fff8a9a5c3a8dc3eda13d3bea76be040b1f8292068276c77c3-%E6%88%AA%E5%B1%8F2019-10-26%E4%B8%8B%E5%8D%8810.35.32.png) 
```javascript
/**
 * @param {string} digits
 * @return {string[]}
 */
var letterCombinations = function(digits) {
    if(!digits){
        return [];
    }
    var len = digits.length;
    var map = new Map();
    map.set('2','abc');
    map.set('3','def');
    map.set('4','ghi');
    map.set('5','jkl');
    map.set('6','mno');
    map.set('7','pqrs');
    map.set('8','tuv');
    map.set('9','wxyz');
    var result = [];
    function _generate(i,str){
        // terminator
        if(i == len){
            result.push(str);
            return;
        }
        // process
        // drill down
        var tmp = map.get(digits[i]);
        for(var r = 0;r<tmp.length;r++){
            _generate(i+1,str+tmp[r]);
        }
    }
    _generate(0,'');
    return result;
};
```
#### 解法二：java版
![截屏2019-10-27上午7.19.22.png](https://pic.leetcode-cn.com/d7d6ae46bcbcc88330ae5ea19dd9ec17e9d5cbc85d0a7fa500b0644b3cada929-%E6%88%AA%E5%B1%8F2019-10-27%E4%B8%8A%E5%8D%887.19.22.png)