# 最大数（中等）
# 题目描述
![截屏2020-01-15下午3.31.38.png](https://pic.leetcode-cn.com/288ad016cdf563387716ae7817b82750b96ef59da9b6f6a08ca9b86fec8f6eac-%E6%88%AA%E5%B1%8F2020-01-15%E4%B8%8B%E5%8D%883.31.38.png)
# 题目地址
<https://leetcode-cn.com/problems/largest-number/>
#### 解法一
+ 拼接对比
+ 结尾判断结果是否是正常的number
```javascript
/**
 * @param {number[]} nums
 * @return {string}
 */
var largestNumber = function(nums) {
    nums.sort( (a,b) => {
        let aStr = a + '';
        let bStr = b + '';
        return (b * Math.pow(10,aStr.length) + a) - (a * Math.pow(10,bStr.length) + b);
    })
    return nums.join('').replace(/^0+/,'') || '0';
};
```
#### 解法二
+ 思路
  + 直觉上，构建最大数字，希望越高位的数字越大越好
    + 降序
  + 关键
    + 首先排序原数组
    + 直接在排序时，对比拼接降序的相邻两个元素后的两个值的大小
      + 大的排前面，如30和3
        + 330 > 303 => 3、30
```javascript
/**
 * @param {number[]} nums
 * @return {string}
 */
var largestNumber = function(nums) {
    nums.sort((a,b) => {
        let t1 = a + '' + b;
        let t2 = b + '' + a;
        if(t1 < t2) return 1;
        else if(t1 > t2) return -1;
        else return 0;
    })
    let ans = nums.join('');
    return ans[0] === '0' ? '0' : ans;
};
```
#### 解法三
+ 只是把拼接过程换成了map(String)
```javascript
/**
 * @param {number[]} nums
 * @return {string}
 */
var largestNumber = function(nums) {
    return nums.map(String).sort((a,b) => (b+a) - (a+b)).join('').replace(/^0+/,'') || '0';
};
```