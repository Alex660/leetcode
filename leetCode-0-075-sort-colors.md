# 颜色分类（中等）
# 题目描述
![截屏2020-01-09上午9.16.32.png](https://pic.leetcode-cn.com/c842e0856919ad61cf1ffef6b0ecb430dd38570b4248be11ff566708ce6a39d9-%E6%88%AA%E5%B1%8F2020-01-09%E4%B8%8A%E5%8D%889.16.32.png)
# 题目地址
<https://leetcode-cn.com/problems/sort-colors/>
#### 解法一：计数排序
+ [思路完全同 - 经典排序算法讲解 - 计数排序](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/BitOperation%E4%BD%8D%E8%BF%90%E7%AE%97%E3%80%81Bloom%20Filter%E5%B8%83%E9%9A%86%E8%BF%87%E6%BB%A4%E5%99%A8%E3%80%81LRU%20Cache%E7%BC%93%E5%AD%98%E3%80%81Sorting%20algorithm%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95.md)
```javascript
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var sortColors = function(nums) {
    let countSort = (arr,maxVal) => {
        let bucketLen = maxVal + 1;
        let bucket = new Array(bucketLen).fill(0);
        let sortedI = 0;
        let arrLen = arr.length;
        for(let i = 0;i < arrLen;i++){
            bucket[arr[i]]++;
        }
        for(let j = 0;j < bucketLen;j++){
            while(bucket[j] > 0){
                arr[sortedI++] = j;
                bucket[j]--;
            }
        }
        return arr;
    }
    return countSort(nums,2);
};
```
#### 解法二：两路替换
```javascript
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var sortColors = function(nums) {
    let left = 0;
    let n = nums.length;
    for(let i = 0;i < n;i++){
        if(nums[i] === 0){
            [nums[left],nums[i]] = [nums[i],nums[left]];
            left++;
        }
    }
    let right = n - 1;
    for(let i = right;i >= left;i--){
        if(nums[i] === 2){
            [nums[right],nums[i]] = [nums[i],nums[right]];
            right--;
        }
    }
};
```
#### 解法三：一次遍历
+ 解法二的 while 版
```javascript
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var sortColors = function(nums) {
    let left = 0;
    let right = nums.length - 1;
    let i = 0;
    while(i <= right){
        if(nums[i] === 0){
            [nums[left],nums[i]] = [nums[i],nums[left]];
            left++;
            i++;
        }
        else if(nums[i] === 2){
            [nums[right],nums[i]] = [nums[i],nums[right]];
            right--;
        }
        else {
            i++;
        }
    }
};
```
+ 或者这样写也可
```javascript
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var sortColors = function(nums) {
    let left = 0;
    let right = nums.length - 1;
    let i = 0;
    while(i <= right){
        while(nums[i] == 2 && i < right){
            [nums[right],nums[i]] = [nums[i],nums[right]];
            right--;
        }
        while(nums[i] == 0 && i > left){
            [nums[left],nums[i]] = [nums[i],nums[left]];
            left++;
        }
        i++;
    }
};
```