# 缺失的第一个正数（困难）
# 题目描述
![截屏2020-01-11上午6.55.44.png](https://pic.leetcode-cn.com/6ee002808d69594d61d4e8aefff7abbc841d0680f84221d0e10b9826ea2df461-%E6%88%AA%E5%B1%8F2020-01-11%E4%B8%8A%E5%8D%886.55.44.png)
# 题目地址
<https://leetcode-cn.com/problems/first-missing-positive/>
#### 解法一：库函数
+ 时间复杂度：O(n^2)
  + 遍历 + 内遍历
+ 空间复杂度：O(1)
  + 没有借助额外存储空间
+ 思路
  + 找数组中缺失的第一个正数
    + 那么一定是从1到n开始寻找，最多n次(即数组的长度)
    + 因此直接利用原数组的下标开始查找
  + includes 方法
    + 用来判断一个数组是否包含一个指定的值，如果包含则返回 true，否则返回false。
    + 实质上需要循环遍历查找，内部时间复杂度为O(n)
  + 如果1～n都出现，那就是n+1
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var firstMissingPositive = function(nums) {
    let n = nums.length;
    for(let i = 1;i <= n;i++){
        if(!nums.includes(i)){
            return i;
        }
    }
    return n + 1;
};
```
#### 解法二：参考解法 - 二分查找
+ [各类算法模板 - 二分查找](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/AlgorithmTemplate%E7%AE%97%E6%B3%95%E6%A8%A1%E6%9D%BF.md)
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var firstMissingPositive = function(nums) {
    let n = nums.length;
    nums = nums.sort((a,b) => a-b)
    let binarySearch = (val) => {
        let left = 0;
        let right = n - 1;
        while(left <= right){
            let mid = (left + right) >> 1;
            if(nums[mid] == val){
                return mid;
            }else if(nums[mid] < val){
                left = mid + 1;
            }else{
                right = mid - 1;
            }
        }
        return -1;
    }
    for(let i = 1;i <= n;i++){
        if(binarySearch(i) == -1){
            return i;
        }
    }
    return n + 1;
};
```
#### 解法三：计数排序 - 经典借助额外空间版
+ 时间复杂度：O(n)
+ 空间复杂度：O(n)
+ [经典排序算法讲解 - 计数排序](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/BitOperation%E4%BD%8D%E8%BF%90%E7%AE%97%E3%80%81Bloom%20Filter%E5%B8%83%E9%9A%86%E8%BF%87%E6%BB%A4%E5%99%A8%E3%80%81LRU%20Cache%E7%BC%93%E5%AD%98%E3%80%81Sorting%20algorithm%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95.md)
+ 思路
  + 解法一：缺失的元素不能超过数组长度+1，所以设置计数数组长度最大为n，即存储的元素最大为n
  + 计数排序后
    + 遍历数组，第二个起为0的元素，说明存在第一个nuns[i] != i的元素
      + 第一个是0，自动忽略
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var firstMissingPositive = function(nums) {
    let n = nums.length;
    let bucket = new Array(n+1).fill(0);
    let sortedIndex = 0;
    for(let i = 0;i <= n;i++){
        let cell = nums[i];
        if(cell <= n){
            bucket[cell]++;
        }
    }
    for(let i = 1;i < bucket.length;i++){
        if(bucket[i] == 0){
            return i;
        }
    }
    return n+1;
};
```
#### 解法四：计数排序 - 原地交换版
+ 时间复杂度：O(n)
+ 空间复杂度：O(1)
+ 思路简析
  + [请先参考 - 经典排序算法讲解 - 计数排序 - 经典版 和 原地交换版实现](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/BitOperation%E4%BD%8D%E8%BF%90%E7%AE%97%E3%80%81Bloom%20Filter%E5%B8%83%E9%9A%86%E8%BF%87%E6%BB%A4%E5%99%A8%E3%80%81LRU%20Cache%E7%BC%93%E5%AD%98%E3%80%81Sorting%20algorithm%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95.md)
  + 其实原地交换，只是借助本身数组进行交换扩张
    + 如果num[i] != nums[nums[i]]，
      + 说明需要像经典版那样以值 num[i]为索引 计数放入原数组，
      + 而因为原数组索引 nums[i] 处的值 nums[nums[i]] 可能有值，为了不漏掉，就需要交换
    + 重复上面交换动作，直至nums[i] <= 0
    + 如果需要对原数组排序，这仅仅适用于原数组没有重复元素时
      + 因为如果有重复元素，你不知道nums[nums[i]]处的值是nums[i]替换过来的，还是本身也等于这个数
      + 但此题仅仅需要找寻缺失的第一个正数，因此重复的没有关系，
        + 只记录一次就好，不需要排序，
        + 所以就不用管重复的元素，直接跳过
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var firstMissingPositive = function(nums) {
    // 省略了 nums.push(0);
    let n = nums.length;
    for(let i = 0;i <= n;i++){
        while(nums[i] > 0 && nums[i] <= n && nums[nums[i]] != nums[i]){
            [nums[nums[i]],nums[i]] = [nums[i],nums[nums[i]]];
        }
    }
    for(let i = 1;i <= n;i++){
        if(nums[i] != i){
            return i;
        }
    }
    return n + 1;
};
```
+ 或者这样写是一样的道理
  + 区别是n存在下标n里还是下标n-1里
  + 本质都是计数排序
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var firstMissingPositive = function(nums) {
    let n = nums.length;
    for(let i = 0;i < n;i++){
        while(nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i]){
            [nums[nums[i] - 1],nums[i]] = [nums[i],nums[nums[i] - 1]];
        }
    }
    for(let i = 0;i < n;i++){
        if(nums[i] != i + 1){
            return i + 1;
        }
    }
    return n + 1;
};
```