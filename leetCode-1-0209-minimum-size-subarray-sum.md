# 长度最小的子数组（中等）
# 题目描述
![截屏2020-01-31下午6.37.26.png](https://pic.leetcode-cn.com/93437cc854081973fa473c2bbe8d464a9156fed007f9c1f1486420c54b24d91a-%E6%88%AA%E5%B1%8F2020-01-31%E4%B8%8B%E5%8D%886.37.26.png)
# 题目地址
<https://leetcode-cn.com/problems/minimum-size-subarray-sum/>
## 二分查找
+ [参考各类算法模板 - 二分查找](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/AlgorithmTemplate%E7%AE%97%E6%B3%95%E6%A8%A1%E6%9D%BF.md)
## 滑动窗口思想
+ 讲解
  + [滑动窗口11道](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/demos/%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A311%E9%81%93.md)
+ 类似题型
  + 1、[3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)
  + 2、[30. 串联所有单词的子串](https://leetcode-cn.com/problems/substring-with-concatenation-of-all-words/)
  + 3、[76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)
  + 4、[159. 至多包含两个不同字符的最长子串](https://leetcode-cn.com/problems/longest-substring-with-at-most-two-distinct-characters/)
  + 5、[209. 长度最小的子数组](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)
  + 6、[239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)
  + 7、[340. 至多包含 K 个不同字符的最长子串](https://leetcode-cn.com/problems/longest-substring-with-at-most-k-distinct-characters/)
  + 8、[438. 找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)
  + 9、[567. 字符串的排列](https://leetcode-cn.com/problems/permutation-in-string/)
  + 10、[632. 最小区间](https://leetcode-cn.com/problems/smallest-range-covering-elements-from-k-lists/)
  + 11、[727. 最小窗口子序列](https://leetcode-cn.com/problems/minimum-window-subsequence/)
+ 戳看👇
  + [leetCode所有题解](https://github.com/Alex660/leetcode)
#### 解法一：通用滑动窗口解法
+ 超出时间限制
  + 通过求和的方式算子数组的长度
```javascript
/**
 * @param {number} s
 * @param {number[]} nums
 * @return {number}
 */
var minSubArrayLen = function(s, nums) {
    let n = nums.length;
    let left = 0,right = 0;
    let match = 0;
    let windows = {};
    let resMax = Number.MAX_SAFE_INTEGER;
    function curry(func){
        var args = [].slice.call(arguments,1);
        var _func =  function(){
            if(arguments.length === 0){
                return func.apply(this,args);
            }else {
                [].push.apply(args,arguments);
                return _func;
            }
        }
        return _func;
    }
    function add(){
        return [].reduce.call(arguments,function(a,b){return a+b});
    }
    while(right < n){
        let c1 = nums[right];
        windows[c1] ? windows[c1]++ : windows[c1] = 1;
        match += c1;
        right++;
        while(match >= s){
            let c2 = nums[left];
            let sum = add(...Object.values(windows)); 
            resMax = Math.min(resMax,sum);
            match -= c2;
            windows[c2]--;
            left++;
        }
    }
    return resMax == Number.MAX_SAFE_INTEGER ? 0 : resMax;
};
```
#### 解法二：解法一优化版
+ 通过下标算子数组长度
```javascript
/**
 * @param {number} s
 * @param {number[]} nums
 * @return {number}
 */
var minSubArrayLen = function(s, nums) {
    let n = nums.length;
    let left = 0,right = 0;
    let match = 0;
    let resMax = Number.MAX_SAFE_INTEGER;
    while(right < n){
        match += nums[right++];
        while(match >= s){
            resMax = Math.min(resMax,right - left);
            match -= nums[left];
            left++;
        }3354-460-2904
    }
    return resMax === Number.MAX_SAFE_INTEGER ? 0 : resMax;
};
```
#### 解法三：暴力法
+ 时间复杂度：O(n^3)
+ 空间复杂度：O(1)
+ 结果
  + 超时
```javascript
/**
 * @param {number} s
 * @param {number[]} nums
 * @return {number}
 */
var minSubArrayLen = function(s, nums) {
    let n = nums.length;
    let res = Number.MAX_SAFE_INTEGER;
    for(let i = 0;i < n;i++){
        for(let j = i;j < n;j++){
            let sum = 0;
            for(let k = i;k <= j;k++){
                sum += nums[k];
            }
            if(sum >= s){
                res = Math.min(res,j - i + 1);
                break;
            }
        }
    }
    return res === Number.MAX_SAFE_INTEGER ? 0 : res;
};
```
#### 解法四：优化的暴力法
+ 时间复杂度：O(n^2)
+ 空间复杂度：O(n)
+ 解法三对每一个子数组求和的时间复杂度为O(n)
+ 此解法优化之处旨在去除这一步，提前存储各个位置的sum总和
  + sums[i]：表示数组中前i个元素的总和
+ 结果
  + 不超时
```javascript
/**
 * @param {number} s
 * @param {number[]} nums
 * @return {number}
 */
var minSubArrayLen = function(s, nums) {
    let n = nums.length;
    if(n == 0) return 0;
    let res = Number.MAX_SAFE_INTEGER;
    let sums = [nums[0]];
    for(let i = 1;i < n;i++){
        sums[i] = sums[i-1] + nums[i];
    }
    for(let i = 0;i < n;i++){
        for(let j = i;j < n;j++){
            let sum = sums[j] - sums[i] + nums[i];
            if(sum >= s){
                res = Math.min(res,j - i + 1);
                break;
            }
        }
    }
    return res === Number.MAX_SAFE_INTEGER ? 0 : res;
};
```
#### 解法五：优化暴力法plus - 二分查找
+ 解法三、四从下标i开始寻找满足sum >= s的子数组需要O(n)的时间
  + 即第二层for循环，时间复杂度为O(n)
  + 既是查找，可以优化为二分查找，时间复杂度为O(logn)
    + 且sums为递增的有序数组，更有利于二分查找
  + 关键
    + 寻找符合 sums[j] - sums[i] >= s2 的子数组
      + s2 == s - nums[i]
        + sums[j] - sums[i] + nums[i] >= s
        + sums[j] - sums[i] >= s - nums[i]
        + sums[j] - sums[i] >= s2
        + sums[j] >= s2 + sums[i]
      + 等同于在二分查找中查找第一个大于 s2 + sums[i] 的值
        + 找到了，说明符合，随机更新最小值
```javascript
/**
 * @param {number} s
 * @param {number[]} nums
 * @return {number}
 */
var minSubArrayLen = function(s, nums) {
    let n = nums.length;
    if(n == 0) return 0;
    let res = Number.MAX_SAFE_INTEGER;
    let sums = [nums[0]];
    for(let i = 1;i < n;i++){
        sums[i] = sums[i-1] + nums[i];
    }
    let binarySearch = (left,right,target) => {
        let mid = -1;
        while(left <= right){
            mid = (left + right) >> 1;
            if(sums[mid] == target){
                return mid;
            }else if(sums[mid] < target){
                left = mid + 1;
            }else{
                right = mid - 1;
            }
        }
        return sums[mid] > target ? mid : -1;
    }
    for(let i = 0;i < n;i++){
        let s2 = s - nums[i];
        let k = binarySearch(i,n-1,s2 + sums[i]);
        if(k != -1){
            res = Math.min(res,k - i + 1);
        }
    }
    return res === Number.MAX_SAFE_INTEGER ? 0 : res;
};
```
#### 解法六：二分法参考解法
+ 思路
  + 1、对于长度为n的数组，判断 n/2 的数组中 是否有符合条件的子数组
  + 2、有，重复第1步
  + 3、没有，增加一半的一半继续第1步判断
```javascript
/**
 * @param {number} s
 * @param {number[]} nums
 * @return {number}
 */
var minSubArrayLen = function(s, nums) {
    let n = nums.length;
    if(n == 0) return 0;
    let minLen = 0,maxLen = n;
    let midLen;
    let minRes = -1;
    let getMaxSum = (len) => {
        let sum = 0;
        let maxSum = 0;
        for(let i = 0;i < len;i++){
            sum += nums[i];
        }
        maxSum = sum;
        for(let i = len;i < n;i++){
            sum += nums[i];
            sum -= nums[i-len];
            maxSum = Math.max(maxSum,sum);
        }
        return maxSum;
    }
    while(minLen <= maxLen){
        midLen = (minLen + maxLen) >> 1;
        if(getMaxSum(midLen) >= s ){
            maxLen = midLen - 1;
            minRes = midLen;
        }else{
            minLen = midLen + 1;
        }
    }
    return minRes == -1 ? 0 : minRes;
};
```