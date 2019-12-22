# 加一（简单）
# 题目描述
![截屏2019-12-22下午2.37.09.png](https://pic.leetcode-cn.com/d167ea9d991c04c387036b8f127394381d06cb491056f61b9d44f7243cbe039c-%E6%88%AA%E5%B1%8F2019-12-22%E4%B8%8B%E5%8D%882.37.09.png)
# 题目地址
<https://leetcode-cn.com/problems/plus-one/>
+ 思路
  + 一个整数加1，无论进不进位，都是从尾部开始加1的
  + 不进位
    + 末尾加1 < 10，加后直接返回，对应数组末尾元素+1即可
  + 进位
    + 对需要进位的原数组中的位的数修改值为0
    + 进位位数 < 整数位数
      + 数组相应位置元素+1即可
      + 何时+1？
        + ==9
        + 即加1后模10为0，说明原位数字为9
    + 进位位数 = 整数位数
      + 例如9，99，999，9999
      + 此时直接变为10，100，1000，10000即可
        + 在遍历中修改
          + 判断是否能遍历到整数的位数，如果能，原数组直接左边插入1个1即可
        + 在结果中修改
          + 直接重新新建一个原数组长度加1长度的新数组，设置首元素为1，其余元素为0，返回新数组即可
#### 解法一：
```javascript
/**
 * @param {number[]} digits
 * @return {number[]}
 */
var plusOne = function(digits) {
    for(let i = digits.length - 1;i >= 0;i--){
        digits[i]++;
        digits[i] = digits[i]%10;
        if(digits[i] != 0) return digits;
    }
    digits = new Array(digits.length+1).fill(0);
    digits[0] = 1;
    return digits;
};
```
#### 解法二：
```javascript
/**
 * @param {number[]} digits
 * @return {number[]}
 */
var plusOne = function(digits) {
    for(let i = digits.length - 1;i >= 0;i--){
        if(digits[i] == 9){
            digits[i] = 0;
        }else{
            digits[i]++;
            return digits;
        }
    }
    digits.unshift(1);
    return digits;
};
```
#### 解法三：
```javascript
/**
 * @param {number[]} digits
 * @return {number[]}
 */
var plusOne = function(digits) {
    let n =digits.length;
    digits[n-1]++;
    for(let i = digits.length - 1;i >= 0;i--){
        if(digits[i] == 10){
            digits[i] = 0;
            if(i == 0){
                digits.unshift(1);
            }else{
                digits[i-1]++;
            }
        }
    }
    return digits;
};
```