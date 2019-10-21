# 两数之和（简单）
# 题目描述
![截屏2019-10-21下午5.13.05.png](https://pic.leetcode-cn.com/cb8479e61958a50ee0859b75eabaaea2ec98d9cfa181c7387c5e1888d8b9ea96-%E6%88%AA%E5%B1%8F2019-10-21%E4%B8%8B%E5%8D%885.13.05.png)
#### 解法一：暴力枚举
+ 时间复杂度 O(n^2)
+ 空间复杂度 O(1)
```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    var result = [];
    var len = nums.length;
    for(var i=0;i<len;i++){
        // j从 i+1 开始 确保不重复利用同一个元素
        for(var j =i+1;j<len;j++){
            if(nums[i] + nums[j] == target){
                result.push(i);
                result.push(j);
                return result;
            }
        }
    }
};
```
#### 解法二：两次哈希表 空间换时间
+ 两个Hash表取值判断
+ 因为不能重复利用同一个元素 所以 Hash[target-val]!=i
```javascript
var twoSum = function(nums, target) {
    var result = new Map();
    var len = nums.length;
    var out = [];
    for(var r=0;r<len;r++){
        result.set(nums[r],r);
    }
    for(var i=0;i<len;i++){
        var tmpDiff=target-nums[i];
        var need = result.get(tmpDiff);
        if(need!=undefined && need!=i){
            out.push(i);
            out.push(need);
            return out;
        }
    }
};
```
#### 解法三：一次哈希表
+ 因为结果元素都源于数组直接找符合差的元素即可
+ 解法二的优化版
```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    var result = new Map();
    var len = nums.length;
    var diff = 0;
    var out = [];
    for(var i=0;i<len;i++){
        diff = target - nums[i];
        var diffVal = result.get(diff);
        if(result.has(diff) && diffVal!=i){
            out.push(i);
            out.push(diffVal);
            return out;
        }else{
            result.set(nums[i],i);
        }
    }
};
```