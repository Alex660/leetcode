# 滑动窗口最大值（困难）
# 题目描述
![截屏2020-02-01下午8.18.41.png](https://pic.leetcode-cn.com/2ebe336b523815f86dce8eaed42eda83ee29ad8511a4696ea3233193bdb56cf7-%E6%88%AA%E5%B1%8F2020-02-01%E4%B8%8B%E5%8D%888.18.41.png)
![截屏2020-02-01下午8.18.48.png](https://pic.leetcode-cn.com/c489037279119ca0a35dd6d82bdc488993011f19960ef35c2db4b659419c871f-%E6%88%AA%E5%B1%8F2020-02-01%E4%B8%8B%E5%8D%888.18.48.png)
# 题目地址
<https://leetcode-cn.com/problems/sliding-window-maximum/>
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
#### 解法一：暴力法
```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
var maxSlidingWindow = function(nums, k) {
    let n = nums.length;
    if(n == 0) return [];
    let res = [];
    for(let i = 0;i < n - k + 1;i++){
        let max = Number.MIN_SAFE_INTEGER;
        for(let j = i;j < i + k;j++){
            max = Math.max(max,nums[j]);
        }
        res.push(max);
    }
    return res;
};
```
#### 解法二：滑动窗口 + 双端队列
+ 时间复杂度：O(N)
  + 每个元素被处理两次- 其索引被添加到双向队列中和被双向队列删除
+ 空间复杂度：O(N)
  + 双向队列的空间
+ 思路
  + [参考题解 - 单调队列解题详解](https://leetcode-cn.com/problems/sliding-window-maximum/solution/dan-diao-dui-lie-by-labuladong/)
  + 填充滑动窗口图解(借用)
    + ![](https://pic.leetcode-cn.com/192b98a80836b47b553ac3b70a13ab3c8dbb1a30f09652f8183f8e4b00c8a3e7-file_1560498372627)
  + 双向队列
    + 维持单调递减队列
      + 每次push元素时，将队列中更小元素删除，直到不小时
      + 每次pop元素时，如果是更小的元素在push时就已经删除了，只需要判断是否是头部最大值，是再删除一遍头部元素即可
```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
var maxSlidingWindow = function(nums, k) {
    let n = nums.length;
    class slideWindow{
        constructor(){
            this.data = [];
        }
        push(val){
            let data = this.data;
            while(data.length > 0 && data[data.length - 1] < val){
                data.pop();
            }
            data.push(val);
        }
        pop(val){
            let data = this.data;
            if(data.length > 0 && data[0] === val){
                data.shift();
            }
        }
        max(){
            return this.data[0];
        }
    }
    let res = [];
    let windows = new slideWindow();
    for(let i = 0;i < n;i++){
        if(i < k - 1){
            windows.push(nums[i]);
        }else{
            windows.push(nums[i]);
            res.push(windows.max());
            windows.pop(nums[i - k + 1]);
        }
    }
    return res;
};
```
#### 解法三：参考解法 - 动态规划
+ 参考题解
  + [中文](https://leetcode-cn.com/problems/sliding-window-maximum/solution/hua-dong-chuang-kou-zui-da-zhi-by-leetcode-3/)
  + [英文](https://leetcode.com/problems/sliding-window-maximum/discuss/65881/O(n)-solution-in-Java-with-two-simple-pass-in-the-array)
```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
var maxSlidingWindow = function(nums, k) {
    let n = nums.length;
    if(n == 0) return [];
    if(k == 1) return nums;
    let res = [];
    let left = new Array(n),right = new Array(n);
    left[0] = nums[0];
    right[n-1] = nums[n-1];
    for(let i = 1;i < n;i++){
        if(i % k == 0) left[i] = nums[i];
        else left[i] = Math.max(left[i-1],nums[i]);
        let j = n - i - 1;
        if((j + 1) % k == 0) right[j] = nums[j];
        else right[j] = Math.max(right[j + 1],nums[j]);
    }
    for(let i = 0;i < n - k + 1;i++){
        res[i] = Math.max(left[i + k - 1],right[i]);
    }
    return res;
};
```