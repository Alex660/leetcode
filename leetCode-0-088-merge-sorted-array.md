# 合并两个有序数组（简单）
# 题目描述
![截屏2019-12-16下午11.32.35.png](https://pic.leetcode-cn.com/9444197adc741f8d74dd1e5434874869fbd821201714aec6c7ae84fc8b99a8ae-%E6%88%AA%E5%B1%8F2019-12-16%E4%B8%8B%E5%8D%8811.32.35.png)
# 题目地址
<https://leetcode-cn.com/problems/merge-sorted-array/>
#### 解法一：双指针 - 从前往后
+ 归并排序
  + 从小到大按顺序缓存到一个数组中
+ nums1按序替换
+ 归并排序不太懂的可以看看我的这篇文章
  + [排序算法](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/BitOperation%E4%BD%8D%E8%BF%90%E7%AE%97%E3%80%81Bloom%20Filter%E5%B8%83%E9%9A%86%E8%BF%87%E6%BB%A4%E5%99%A8%E3%80%81LRU%20Cache%E7%BC%93%E5%AD%98%E3%80%81Sorting%20algorithm%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95.md#%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95)
```javascript
/**
 * @param {number[]} nums1
 * @param {number} m
 * @param {number[]} nums2
 * @param {number} n
 * @return {void} Do not return anything, modify nums1 in-place instead.
 */
var merge = function(nums1, m, nums2, n) {
    let left = 0;
    let right = 0;
    let tmp_nums1 = nums1.slice(0,m);
    let tmp_nums2 = nums2.slice(0,n);
    let result = [];
    while(left < m && right < n){
        if(tmp_nums1[left] < tmp_nums2[right]){
            result.push(tmp_nums1[left]);
            left++;
        }else{
            result.push(tmp_nums2[right]);
            right++;
        }
    }
    result = result.concat(tmp_nums1.slice(left)).concat(tmp_nums2.slice(right));
    for(let i = 0;i < result.length;i++){
        nums1[i] = result[i];
    }
};
```
#### 解法二：双指针 + 从后向前
+ 思路
  + 两个数组从小到大排序
  + 且题目要求 修改nums1为合并排好序的nums1+nums2
  + 双指针
    + 两个分别指向两个数组尾部的指针
  + 从后向前
    + 比较两指针位置的值
      + 大的一定是结果数组的最大值
    + 一一填充到 nums1的末尾
  + 遍历完后
    + 当 n > 0 时
      + 说明  nums2 中还有剩余没有比较的数字
      + 将其插入替换 nums1 数组前面n个数字即可
```javascript
/**
 * @param {number[]} nums1
 * @param {number} m
 * @param {number[]} nums2
 * @param {number} n
 * @return {void} Do not return anything, modify nums1 in-place instead.
 */
var merge = function(nums1, m, nums2, n) {
    let count = m + n;
    while(m > 0 && n > 0){
        nums1[--count] = nums1[m-1] < nums2[n-1] ? nums2[--n] : nums1[--m];
    }
    if(n > 0){
        nums1.splice(0,n,...nums2.slice(0,n));
    }
};
```