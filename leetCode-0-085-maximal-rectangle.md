# 最大矩形（困难）
# 题目描述
![截屏2019-12-05上午5.32.05.png](https://pic.leetcode-cn.com/a35e0f4f7ddd99ba9d31a4c79656b91ad07df9a755d9be0914de4f0d55fc95fd-%E6%88%AA%E5%B1%8F2019-12-05%E4%B8%8A%E5%8D%885.32.05.png)
# 题目地址
<https://leetcode-cn.com/problems/maximal-rectangle/>
#### 解法一：栈解
+ 类似题型
  + [84. 柱状图中最大的矩形-解法六-未优化版](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/solution/84-zhu-zhuang-tu-zhong-zui-da-de-ju-xing-by-alexer/)
+ 思路
  + 遍历每一行的高度
  + 用84题求最大矩形的方法直接套过来求解即可
  + 图解
    + ![截屏2019-12-12上午7.23.20.png](https://pic.leetcode-cn.com/1b5a2903f6a9c54c1c1db3b5389a4151f8b7239d92b845729c2f3c674507b5a4-%E6%88%AA%E5%B1%8F2019-12-12%E4%B8%8A%E5%8D%887.23.20.png)
```javascript
/**
 * @param {character[][]} matrix
 * @return {number}
 */
var maximalRectangle = function(matrix) {
    let max = 0;
    if(!matrix || matrix.length == 0){
        return 0;
    }
    let largestRectangleArea = function(heights) {
        let stack = [-1];
        for(let i = 0;i < heights.length;i++){
            while(stack.length > 1 && heights[stack[stack.length-1]] >= heights[i]){
                maxarea = Math.max(maxarea,heights[stack.pop()] * (i - stack[stack.length-1] - 1));
            }
            stack.push(i);
        }
        while(stack.length > 1){
            maxarea = Math.max(maxarea,heights[stack.pop()] * (heights.length - stack[stack.length-1] - 1));
        }
    };
    let row = matrix.length;
    let col = matrix[0].length;
    let maxarea = 0;
    let heights = new Array(col).fill(0);
    for(let i = 0;i < row;i++){
        for(let j = 0;j < col;j++){
            if(matrix[i][j] == '1'){
                heights[j] += 1;
            }else{
                heights[j] = 0;
            }
        }
        largestRectangleArea(heights);
    }
    return maxarea;
};
```
#### 解法二：栈 + 动态规划
```javascript
/**
 * @param {character[][]} matrix
 * @return {number}
 */
var maximalRectangle = function(matrix) {
    let max = 0;
    if(!matrix || matrix.length == 0){
        return 0;
    }
    let largestRectangleArea = function(heights) {
        let stack = [-1];
        for(let i = 0;i < heights.length;i++){
            while(stack.length > 1 && heights[stack[stack.length-1]] >= heights[i]){
                maxarea = Math.max(maxarea,heights[stack.pop()] * (i - stack[stack.length-1] - 1));
            }
            stack.push(i);
        }
        while(stack.length > 1){
            maxarea = Math.max(maxarea,heights[stack.pop()] * (heights.length - stack[stack.length-1] - 1));
        }
    };
    let row = matrix.length;
    let col = matrix[0].length;
    let maxarea = 0;
    for(let i = 0;i < row;i++){
        for(let j = 0;j < col;j++){
            if(matrix[i][j] == '1'){
                matrix[i][j] = i === 0 ? 1 : Number( matrix[i - 1][j] ) + 1;
            }
        }
        largestRectangleArea(matrix[i]);
    }
    return maxarea;
};
```
+ 或者这样写更好理解
```javascript
/**
 * @param {character[][]} matrix
 * @return {number}
 */
var maximalRectangle = function(matrix) {
    let max = 0;
    if(!matrix || matrix.length == 0){
        return 0;
    }
    let largestRectangleArea = function(heights) {
        let stack = [-1];
        for(let i = 0;i < heights.length;i++){
            while(stack.length > 1 && heights[stack[stack.length-1]] >= heights[i]){
                maxarea = Math.max(maxarea,heights[stack.pop()] * (i - stack[stack.length-1] - 1));
            }
            stack.push(i);
        }
        while(stack.length > 1){
            maxarea = Math.max(maxarea,heights[stack.pop()] * (heights.length - stack[stack.length-1] - 1));
        }
    };
    let row = matrix.length;
    let col = matrix[0].length;
    let maxarea = 0;
    for(let j = 0;j < col;j++){
        if(matrix[0][j] == '1'){
            matrix[0][j] = 1;
        }
    }
    largestRectangleArea(matrix[0]);
    for(let i = 1;i < row;i++){
        for(let j = 0;j < col;j++){
            if(matrix[i][j] == '1'){
                matrix[i][j] = Number(matrix[i - 1][j]) + 1;
            }
        }
        largestRectangleArea(matrix[i]);
    }
    return maxarea;
};
```