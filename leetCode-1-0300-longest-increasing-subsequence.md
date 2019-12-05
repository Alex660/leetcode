# 最长上升子序列（中等）
# 题目描述
![截屏2019-12-03上午9.59.35.png](https://pic.leetcode-cn.com/ddb1c0fb5b9e43e3468e531978011fab71ecadad88e4ca67bbc1e9103a643d22-%E6%88%AA%E5%B1%8F2019-12-03%E4%B8%8A%E5%8D%889.59.35.png)
# 题目地址
<https://leetcode-cn.com/problems/longest-increasing-subsequence/>
#### 解法一：动态规划
+ 时间复杂度：O(n^2)
+ 空间复杂度：O(n)
+ 思路分析
  + 图解
    + ![截屏2019-12-03下午5.50.07.png](https://pic.leetcode-cn.com/d6f72f4dcd96ffd7a978d2371e57a81f8cbc8e05250122f7324af926691bad63-%E6%88%AA%E5%B1%8F2019-12-03%E4%B8%8B%E5%8D%885.50.07.png)
  + 状态定义
    + dp[i]：**表示以nums[i]为当前最长递增子序列尾元素的长度**
  + 转移方程
    + 当 nums[i] > nums[j] 时，
      + nums[i]可以作为前1个是最长的递增子序列 dp[j] 新的尾元素，
      + 而组成新的相对于dp[i]能够拼接的更长的递增子序列：dp[i] = dp[j] + 1；
        + 如图：dp[0] 和 dp[1]
      + 因为新的dp[i]能够拼接的最长长度取决于nums[i]这个新的尾元素，而这个nums[i]不一定大于nums[j]，所以也不一定大于dp[j]
        + 比如dp[j] = 4，nums[i] < nums[j] && nums[i] > nums[z] && dp[z] = 2 && z < i && j < i
        + 则此时，dp[i] = dp[z] + 1 = 3，不能拼接在dp[j]的后面，不是一定的
        + 如图 dp[3] > dp[4] && nums[4] < nums[3] 
      + 那么在i~j之间，最大的递增子序列 = Max(dp[i],dp[j]+1)
      + 即方程为：**dp[i] = Max(dp[i],dp[j]+1)**
    + 当 nums[i] <= nums[j] 时，
      + 是下降了，不满足上升，跳过继续遍历下一个
  + 由此可知，每一阶段都有相邻两个递增子序列，那么所有递增子序列的最大值为：
    + 1、求解过程中，维护一个变量max，如当前解法中第一种写法
    + 2、在最后for循环遍历求最大值或者用库函数求数组中的最大值，如当前解法中第二种写法
  + 初始化
    + 为1，每个元素本身也是一个子序列，长度为1
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var lengthOfLIS = function(nums) {
    let n = nums.length;
    if(n == 0){
        return 0;
    }
    let dp = new Array(n).fill(1);
    let max = 0;
    for(let i = 0;i < n;i++){
        for(let j = 0;j < i;j++){
            if(nums[j] < nums[i]){
                dp[i] = Math.max(dp[i],dp[j]+1);
            }
        }
        max = Math.max(max,dp[i]);
    }
    return max;
};
```
+ 或者这样写
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var lengthOfLIS = function(nums) {
    let n = nums.length;
    if(n == 0){
        return 0;
    }
    let dp = new Array(n).fill(1);
    for(let i = 0;i < n;i++){
        for(let j = 0;j < i;j++){
            if(nums[j] < nums[i]){
                dp[i] = Math.max(dp[i],dp[j]+1);
            }
        }
    }
    // return Math.max(...dp);
    return Math.max.apply(null,dp);
};
```
#### 解法二：二分查找
+ 思路分析
  + 此解法和解法一相同的是，转移方程推导那里当 nums[i] > nums[j] 时的第一种情况，
  + 唯一不同的是第二种情况
  + 区别
    + 解法一那里是双层for循环来一一遍历每个dp[k]，时间复杂度为O(n^2)
    + 这里我们考虑降至O(logN)，因而想到二分查找
    + 再看解法一那张图的示例规律
      + [1]     => dp[0] = 1
      + [1,4]   => dp[1] = 2
      + [1,3]   => dp[2] = 2
      + [1,3,4] => do[3] = 3
      + [1,2]   => dp[4] = 2
    + 当 nums[i] > nums[j] 时
      + 直接将nums[i]拼接到dp[j]后面形成新的dp[i] = dp[j] + 1
    + 当 nums[i] <= nums[j] 时
      + 将nums[i] 替换dp[j] 对应的数组中第一个大于nums[i]的数如nums[r]
        + 那么问题来了，怎么找呢？二分查找！
    + 总结
      + 当前遍历元素 x 大于 前一个递增子序列的 尾元素时
        + 追加到后面
      + 否则
        + 寻找前一个递增子序列第一个大于当前值的元素，替换为当前值
        + 查找用二分，最后左边的元素即为查找到的需要被替换的结果元素
    + 注意
      + 经大量证明，被替换后形成的新的数组未必是解法一中正确结果的数组，但长度是一样的
      + 证明涉及数学，较为复杂，略
        + 可以这么想，假设手里有一副牌，每次抓牌上手时，都会对牌进行从小到大排序
          + 遇到大的，排在后面
          + 遇到小的，要搜索排好序的牌里的一个位置插入使其依然有序
            + 而查找的过程是二分，插入的位置相当于把原来牌的位置占据
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var lengthOfLIS = function(nums) {
    let n = nums.length;
    if(n <= 1){
        return n;
    }
    let tail = new Array(n);
    tail[0] = nums[0];
    let end = 0;
    for(let i = 1;i < n;i++){
        if(nums[i] > tail[end]){
            end++;
            tail[end] = nums[i];
        }else{
            let left = 0;
            let right = end;
            while(left < right){
                let mid = left + ((right - left) >> 1);
                if(tail[mid] < nums[i]){
                    left = mid + 1;
                }else{
                    right = mid;
                }
            }
            tail[left] = nums[i];
        }
    }
    return end + 1;
};
```
+ 或者这样写
  + 只是把上面nums[i] > tail[end]的情况糅合进里面了
    + 通过索引的方式判断
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var lengthOfLIS = function(nums) {
    let n = nums.length;
    if(n <= 1){
        return n;
    }
    let tail = new Array(n);
    let end = 0;
    for(let i = 0;i < n;i++){
        let left = 0;
        let right = end;
        while(left < right){
            let mid = (left + right) >> 1;
            if(tail[mid] < nums[i]){
                left = mid + 1;
            }else{
                right = mid;
            }
        }
        tail[left] = nums[i];
        end == right && end++
    }
    return end;
};
```
+ 或者这样写
  + 既然更新的是数组，可以更新递增的索引来求长度
    + 那么也可以通过递增数组本身长度来求，是一样的
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var lengthOfLIS = function(nums) {
    let n = nums.length;
    if(n <= 1){
        return n;
    }
    let tail = [nums[0]];
    for(let i = 0;i < n;i++){
        if(nums[i] > tail[tail.length-1]){
            tail.push(nums[i]);
        }else{
            let left = 0;
            let right = tail.length-1;
            while(left < right){
                let mid = (left + right) >> 1;
                if(tail[mid] < nums[i]){
                    left = mid + 1;
                }else{
                    right = mid;
                }
            }
            tail[left] = nums[i];
        }
    }
    return tail.length;
};
```