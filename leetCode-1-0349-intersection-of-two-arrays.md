# 两个数组的交集（简单）
# 题目描述
![截屏2020-01-11上午6.55.44.png](https://pic.leetcode-cn.com/6842741da1b88ed76c593c673f91a00db5fe68e13cb3fe980eb533c361ceb331-%E6%88%AA%E5%B1%8F2020-01-11%E4%B8%8A%E5%8D%886.55.44.png)
# 题目地址
<https://leetcode-cn.com/problems/intersection-of-two-arrays/>
#### 解法一：哈希去重 + 库函数
```javascript
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersection = function(nums1, nums2) {
    let tmp = {};
    return nums1.filter((item) => {
        if(!tmp[item] && nums2.includes(item)){
            tmp[item] = true;
            return nums2.includes(item)
        }
    })
};
```
#### 解法二：全库函数
```javascript
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersection = function(nums1, nums2) {
    return [...new Set(nums1.filter((item) => {
        return nums2.includes(item)
    }))]
};
```
#### 解法三：哈希
+ 如果 nums1 的大小比 nums2 小很多，此种方法更优
+ 时间复杂度：O(n + m)
  + n,m代表数组大小
+ 空间复杂度：O(min(n,m))
  + 对较小的数组进行哈希映射使用的空间
```javascript
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersection = function(nums1, nums2) {
    if(nums1.length > nums2.length) [nums1,nums2] = [nums2,nums1];
    let hash = new Set(nums1);
    let res = new Set();
    for(let i = 0;i < nums2.length;i++){
        if(hash.has(nums2[i])){
            res.add(nums2[i]);
        }
    }
    return [...res];
};
```
#### 解法四：排序 + 双指针
+ 时间复杂度：O(nlogn)
```javascript
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersection = function(nums1, nums2) {
    nums1 = nums1.sort((a,b) => a - b);
    nums2 = nums2.sort((a,b) => a - b);
    let i = 0;
    let j = 0;
    let res = new Set();
    while(i < nums1.length && j < nums2.length){
        if(nums1[i] < nums2[j]){
            i++;
        }else if(nums1[i] > nums2[j]){
            j++;
        }
        else{
            res.add(nums1[i]);
            i++;
            j++;
        }
    }
    return [...res];
};
```
#### 解法五：二分查找
+ 时间复杂度：O(nlogn)
+ [参考各类算法模板 - 二分查找](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/AlgorithmTemplate%E7%AE%97%E6%B3%95%E6%A8%A1%E6%9D%BF.md)
```javascript
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersection = function(nums1, nums2) {
    let res = new Set();
    nums2 = nums2.sort((a,b) => a - b);
    let binarySearch = (arr,val) => {
        let left = 0;
        let right = arr.length - 1;
        while(left <= right){
            let mid = (left + right) >> 1;
            if(arr[mid] === val){
                return true;
            }else if(arr[mid] > val){
                right = mid - 1;
            }else{
                left = mid + 1;
            }
        }
        return false;
    }
    for(let i = 0;i < nums1.length;i++){
        if(binarySearch(nums2,nums1[i])){
            res.add(nums1[i]);
        }
    }
    return [...res];
};
```
#### 解法六：库函数 + ES6
```javascript
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersection = function(nums1, nums2) {
    return [...new Set([...new Set(nums1)].filter(x => new Set(nums2).has(x)))];
};
```