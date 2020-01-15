# 最大间距（困难）
# 题目描述
![截屏2020-01-11下午9.19.47.png](https://pic.leetcode-cn.com/38d7dee56539c77ced36886ddfc4ba4f3acece2474144572b80f9fc1ad1dd661-%E6%88%AA%E5%B1%8F2020-01-11%E4%B8%8B%E5%8D%889.19.47.png)
# 题目地址
<https://leetcode-cn.com/problems/maximum-gap/>
#### 解法一：库函数排序
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maximumGap = function(nums) {
    let n = nums.length;
    if(n < 2) return 0;
    nums = nums.sort((a,b) => a - b);
    let max = -1;
    for(let j = 1;j < n;j++){
        let val = nums[j] - nums[j-1];
        if(val > max){
            max = val;
        }
    }
    return max;
};
```
#### 解法二：桶 + 鸽笼原理
+ [经典排序算法讲解 - 桶排序](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/BitOperation%E4%BD%8D%E8%BF%90%E7%AE%97%E3%80%81Bloom%20Filter%E5%B8%83%E9%9A%86%E8%BF%87%E6%BB%A4%E5%99%A8%E3%80%81LRU%20Cache%E7%BC%93%E5%AD%98%E3%80%81Sorting%20algorithm%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95.md)
+ [鸽笼原理参考官方解释](https://leetcode-cn.com/problems/maximum-gap/solution/zui-da-jian-ju-by-leetcode/)
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maximumGap = function(nums) {
    let n = nums.length;
    if(n < 2) return 0 ;
    let min = Math.min(...nums);
    let max = Math.max(...nums);
    if( max - min == 0) return 0;
    let gap = Math.ceil((max - min) / (n - 1));
    let bucketsMin = new Array(n - 1).fill(Number.MAX_SAFE_INTEGER);
    let bucketsMax = new Array(n - 1).fill(-1);
    for(let i = 0;i < nums.length;i++){
        if(nums[i] == min || nums[i] == max) continue;
        let idx = parseInt((nums[i] - min) / gap);
        bucketsMin[idx] = Math.min(nums[i],bucketsMin[idx]);
        bucketsMax[idx] = Math.max(nums[i],bucketsMax[idx]);
    }
    let maxGap = 0;
    let pre = min;
    for(let i = 0;i < n - 1;i++){
        if(bucketsMax[i] == -1) continue;
        maxGap = Math.max(maxGap,bucketsMin[i] - pre);
        pre = bucketsMax[i];
    }
    maxGap = Math.max(maxGap,max - pre);
    return maxGap;
};
```
#### 解法三：基数排序
+ [经典排序算法讲解 - 基数排序](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/BitOperation%E4%BD%8D%E8%BF%90%E7%AE%97%E3%80%81Bloom%20Filter%E5%B8%83%E9%9A%86%E8%BF%87%E6%BB%A4%E5%99%A8%E3%80%81LRU%20Cache%E7%BC%93%E5%AD%98%E3%80%81Sorting%20algorithm%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95.md)
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maximumGap = function(nums) {
    let n = nums.length;
    if(n < 2) return 0 ;
    let maxDigit = Math.max(...nums).toString().length;
    let radixSort = (arr,maxDigit) => {
        let digit = 1;
        let mod = 10;
        let bucket = new Array(10);
        for(let i = 0;i < maxDigit;i++){
            for(let j = 0;j < arr.length;j++){
                let index = Math.floor(arr[j]/digit) % mod;
                if(bucket[index] == null) bucket[index] = [];
                bucket[index].push(arr[j]);
            }
            let pos = 0;
            for(let r = 0;r < bucket.length;r++){
                let val = null;
                if(bucket[r]){
                    while((val = bucket[r].shift()) != null){
                        arr[pos++] = val;
                    }  
                }
            }
            digit *= 10;
        }
    }
    radixSort(nums,maxDigit);
    let max = -1;
    for(let j = 1;j < n;j++){
        let val = nums[j] - nums[j-1];
        if(val > max){
            max = val;
        }
    }
    return max;
};
```