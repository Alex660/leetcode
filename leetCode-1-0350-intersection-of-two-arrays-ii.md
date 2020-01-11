#  两个数组的交集 II（简单）
# 题目描述
![截屏2020-01-11下午6.51.42.png](https://pic.leetcode-cn.com/10fca55664ccb2db004598601f72eca5aa47bd8b68ed2508a7fd5b98341c40ef-%E6%88%AA%E5%B1%8F2020-01-11%E4%B8%8B%E5%8D%886.51.42.png)
# 题目地址
<https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/>
#### 解法一：对应进阶一 ：排序 + 双指针
+ [思路同 349. 两个数组的交集 - 解法四](https://leetcode-cn.com/problems/intersection-of-two-arrays/solution/349-liang-ge-shu-zu-de-jiao-ji-by-alexer-660/)
+ 区别是不能去重， 重复的相同元素也要
```javascript
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersect = function(nums1, nums2) {
    nums1 = nums1.sort((a,b) => a - b);
    nums2 = nums2.sort((a,b) => a - b);
    let i = 0;
    let j = 0;
    let res = [];
    while(i < nums1.length && j < nums2.length){
        if(nums1[i] < nums2[j]){
            i++;
        }else if(nums1[i] > nums2[j]){
            j++;
        }
        else{
            res.push(nums1[i]);
            i++;
            j++;
        }
    }
    return res;
};
```
#### 解法二：对应进阶二 ：哈希
+ 时间复杂度：O(n+m)
+ 空间复杂度：O(min(n,m))
+ [思路同 349. 两个数组的交集 - 解法三](https://leetcode-cn.com/problems/intersection-of-two-arrays/solution/349-liang-ge-shu-zu-de-jiao-ji-by-alexer-660/)
+ 区别是不能去重， 重复的相同元素也要
  + 对应哈希，就是对重复的元素进行计数
  + 返回
    + 不需额外设置存储空间，直接对nums1进行覆盖，最后返回相应元素的数即可
```javascript
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersect = function(nums1, nums2) {
    if(nums1.length > nums2.length) [nums1,nums2] = [nums2,nums1];
    let hash = {};
    for(let i = 0;i < nums1.length;i++){
        if(hash[nums1[i]]){
            hash[nums1[i]]++;
        }else{
            hash[nums1[i]] = 1;
        }
    }
    let r = 0;
    for(let i = 0;i < nums2.length;i++){
        if(hash[nums2[i]]){
            nums1[r++] = nums2[i];
            hash[nums2[i]]--;
        }
    }
    return nums1.slice(0,r);
};
```
#### 解法三：对应进阶三 ：双指针 + 归并排序
+ 其实就是将解法一定义的res去掉
  + 换做和解法二一样，不利用额外空间
```javascript
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersect = function(nums1, nums2) {
    nums1 = nums1.sort((a,b) => a - b);
    nums2 = nums2.sort((a,b) => a - b);
    let i = 0;
    let j = 0;
    let k = 0;
    while(i < nums1.length && j < nums2.length){
        if(nums1[i] < nums2[j]){
            i++;
        }else if(nums1[i] > nums2[j]){
            j++;
        }
        else{
            nums1[k++] = nums1[i];
            i++;
            j++;
        }
    }
    return nums1.slice(0,k);
};
```