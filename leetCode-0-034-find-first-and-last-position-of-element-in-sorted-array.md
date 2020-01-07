# 在排序数组中查找元素的第一个和最后一个位置（中等）
# 题目描述
![截屏2020-01-07上午6.21.42.png](https://pic.leetcode-cn.com/2a65358d7d0679f76713ba3b7e3c902dc59f211aec990b5a4f4b006b7292598d-%E6%88%AA%E5%B1%8F2020-01-07%E4%B8%8A%E5%8D%886.21.42.png)
# 题目地址
<https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/>
## 各种算法模板
+ [算法模板](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/AlgorithmTemplate%E7%AE%97%E6%B3%95%E6%A8%A1%E6%9D%BF.md)
#### 解法：二分查找
```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var searchRange = function(nums, target) {
    let n = nums.length;
    if(n == 0) return [-1,-1];
    let leftBound = () => {
        let left = 0;
        let right = n;
        while(left < right){
            let mid = (left + right) >> 1;
            if(nums[mid] == target){
                right = mid;
            }else if(nums[mid] < target){
                left = mid + 1;
            }else if(nums[mid] > target){
                right = mid;
            }
        }
        if(nums[left] != target) return -1;
        return left;
    }
    let rightBound = () => {
        let left = 0;
        let right = n;
        while(left < right){
            let mid = (left + right) >> 1;
            if(nums[mid] == target){
                left = mid + 1; 
            }else if(nums[mid] < target){
                left = mid + 1;
            }else if(nums[mid] > target){
                right = mid;
            }
        }
        return left - 1;
    }
    let left = leftBound();
    if(left == -1) return [-1,-1];
    let right = rightBound();
    return [left,right];
};
```