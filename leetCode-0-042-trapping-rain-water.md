# 接雨水（困难）
# 题目描述
![截屏2019-12-13上午6.59.50.png](https://pic.leetcode-cn.com/ee27809b2f2ffd2b1b0d43715bdf38bdc8758c5517fb943c9ab9d46515df3f40-%E6%88%AA%E5%B1%8F2019-12-13%E4%B8%8A%E5%8D%886.59.50.png)
# 题目地址
<https://leetcode-cn.com/problems/trapping-rain-water/>
#### 解法一：按列求和
+ 时间复杂度：O(n^2)
+ 空间复杂度：O(1)
+ 木桶效应
  + 最短的那块木板决定木桶中水的深度
+ **水洼效应**【自定义，莫笑】
  + 水洼的储水高度 = 两边高地(洼滩)中最矮的那边 - 水洼洼底本身的高度
  + 水洼面积 = 储水高度 * 水洼宽度
    + 应用于此题，每列宽度恒为1，相当于水洼洼底平坦，木有坑！
+ 思路
  + 从左到右 遍历
  + 设当列柱子为水洼洼底
  + 同时更新，**当前列左右两边**洼滩的最大高度
  + 以当前列为洼底的最大水洼面积为
    + ( Min(left_max,right_max) - height[i] ) * 1
```javascript
/**
 * @param {number[]} height
 * @return {number}
 */
var trap = function(height) {
    if(!height || height.length == 0){
        return 0;
    }
    let sum = 0;
    // 两端列相当于只有洼底没有洼滩不能储水，也相当于平地
    // 因而下标不包含0、height.length - 2
    for(let i = 1;i < height.length - 1;i++){
        let max_left = 0;
        // 当前洼底左边最大的洼滩高度
        for(let j = i - 1;j >= 0;j--){
            max_left = Math.max(max_left,height[j]);
        }
        let max_right = 0;
        // 当前洼底右边最大的洼滩高度
        for(let j = i + 1;j < height.length;j++){
            max_right = Math.max(max_right,height[j])
        }
        // 求能够储水的最大高度
        let min = Math.min(max_left,max_right);
        if(min >  height[i]){
            sum += min - height[i];
        }
    }
    return sum;
};
```
#### 解法二：动态规划 + 按列求和
+ 解法一中枚举每一列来求和
  + 期间，每次都要再重新枚举当前列左右两边的最大高度
+ 状态定义
  + left_max[i]：当前第i列左边最高的柱子的高度
  + right_max[i]：当前第i列右边最高的柱子的高度
+ 转移方程
  + left_max[i] = Max(left_max[i-1],height[i-1])
  + right_max[i] = Max(right_max[i+1],height[i+1])
+ 按列求和
  + sum += (Min(left_max[i],right_max[i]) - height[i]) * 1;
```javascript
/**
 * @param {number[]} height
 * @return {number}
 */
var trap = function(height) {
    if(!height || height.length == 0){
        return 0;
    }
    let sum = 0;
    let n = height.length;
    let left_max = new Array(n);
    let right_max = new Array(n);
    left_max[0] = height[0];
    right_max[n-1] = height[n-1];
    for(let i = 1;i < n;i++){
        left_max[i] = Math.max(left_max[i-1],height[i]);
    }
    for(i = n - 2;i >= 0;i--){
        right_max[i] = Math.max(right_max[i+1],height[i]);
        sum += Math.min(left_max[i],right_max[i]) - height[i];
    }
    return sum;
};
```
#### 解法三：动态规划降维[双指针] + 按列求和
+ left_max，right_max无需定义数组存储
  + 每次只用到当前列前一个左右的最大的高度
  + 直接维护更新两个变量即可
```javascript
/**
 * @param {number[]} height
 * @return {number}
 */
var trap = function(height) {
    if(!height || height.length == 0){
        return 0;
    }
    let sum = 0;
    let n = height.length;
    let left = 0;
    let right = n - 1;
    let left_max = 0;
    let right_max = 0;
    while(left < right){
        if(height[left] < height[right]){
            if(height[left] < left_max){
                sum += left_max - height[left];
            }else{
                left_max = height[left];
            }
            left++;
        }else{
            if(height[right] < right_max){
                sum += right_max - height[right];
            }else{
                right_max = height[right];
            }
            right--;
        }
    }
    return sum;
};
```
#### 解法四：栈 + 按列求和
+ 类似题型
  + [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/solution/84-zhu-zhuang-tu-zhong-zui-da-de-ju-xing-by-alexer/)
+ 思路
  + 栈
    + 与84题不同的是，这里维护一个单调递减的栈
    + 例如[8,5,4,3]，当遍历到下一个数字为6时，说明遇到上升的了
  + 按列求和
    + 这时要累加（（6-左边出栈的高度）* 区间长度即宽 ）
      + 只有当前高度比两边高度都小，才能盛水，如水洼一样
    + 注意
      + 只不过这里的列不止一列，每次列的宽度不只是解法一中的长度1
      + 跟以上解法不同的是，水洼的宽度不再默认是1，而是n
+ 栈的具体思路可参看84题
```javascript
/**
 * @param {number[]} height
 * @return {number}
 */
var trap = function(height) {
    if(!height || height.length == 0){
        return 0;
    }
    let stack = [];
    let res = 0;
    for(let i = 0;i < height.length;i++){
        while(stack.length != 0 && height[stack[stack.length-1]] < height[i]){
            let tmp = stack.pop();
            if(stack.length == 0){
                break;
            }
            res += (Math.min(height[i],height[stack[stack.length-1]]) - height[tmp]) * (i - stack[stack.length-1] - 1);
        }
        stack.push(i);
    }
    return res;
};
```