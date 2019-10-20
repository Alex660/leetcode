# 盛最多水的容器（中等）
# 题目地址
<https://leetcode-cn.com/problems/container-with-most-water/solution/>
#### 解法一：暴力枚举
+ 时间复杂度:O(n^2)<=双重循环【计算1,2,3,4...n中高度组合的面积为 n(n-1)/2  】
+ 空间复杂度:O(1)
```javascript
var maxArea = function(height) {    
    var maxarea = 0;
    for(var i = 0;i<height.length;i++){
        for(var r = i +1;r<height.length;r++){
            maxarea = Math.max(maxarea,Math.min(height[i],height[r])*(r-i))
        }
    }
    return maxarea;
}
```
#### 解法二：双指针法
+ 两线段之间形成的区域总是受到其中较短那条长度的限制 && 两线段距离越远，得到面积越大
+ 设置首尾两个指针；每次遍历，将高度短的指针向中间靠拢，如果将长的指针向中间靠拢则面积一定缩小，但移动短但一端虽然长度减小但是有着遇到更高高度使得面积增加但可能性！
+ 时间复杂度:O(n),一次扫描
+ 空间复杂度:O(1)
```javascript
/**
 * @param {number[]} height
 * @return {number}
 */
var maxArea = function(height) {    
    var maxarea = 0;
    var start = 0,end = height.length-1;
    while(start<end){
        maxarea = Math.max(maxarea,Math.min(height[start],height[end])*(end-start));
        if(height[start]>=height[end]){
            end--;
        }else{
            start++;
        }
    }
    return maxarea;
};
```