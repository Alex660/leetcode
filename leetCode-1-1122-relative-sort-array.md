# 数组的相对排序（简单）
# 题目描述
![截屏2019-11-27下午5.11.02.png](https://pic.leetcode-cn.com/613eb03934730a2cdb71871599a4f501d1de8f1f5ca1fe0c405b64dba007336e-%E6%88%AA%E5%B1%8F2019-11-27%E4%B8%8B%E5%8D%885.11.02.png)
# 题目地址
<https://leetcode-cn.com/problems/relative-sort-array/>
#### 解法一：计数排序 A
+ 将输入的数据值转化为键存储在额外开辟的数组空间中；
+ 然后依次把计数大于 1 的填充回原数组
  + 此题填充过程中有所不同
    + 先把arr2出现的元素对应的键值依次取出
    + 最后把计数数组里剩余的数据依次取出
```javascript
/**
 * @param {number[]} arr1
 * @param {number[]} arr2
 * @return {number[]}
 */
var relativeSortArray = function(arr1, arr2) {
    let maxValue = Math.max(...arr1);
    let bucket = new Array(maxValue+1).fill(0);
    let result = [];
    for(let i = 0;i < arr1.length;i++){
        bucket[arr1[i]]++;
    }
    for(let j = 0;j < arr2.length;j++){
        while(bucket[arr2[j]] > 0){
            result.push(arr2[j]);
            bucket[arr2[j]]--;
        }
    }
    for(let r = 0;r <= maxValue;r++){
        while(bucket[r] > 0){
            result.push(r);
            bucket[r]--;
        }
    }
    return result;
};
```
#### 解法二：计数排序 B
```javascript
/**
 * @param {number[]} arr1
 * @param {number[]} arr2
 * @return {number[]}
 */
var relativeSortArray = function(arr1, arr2) {
    let bucket = {};
    let result = [];
    for(let i = 0;i < arr1.length;i++){
        if(bucket[arr1[i]]){
            bucket[arr1[i]]++;
        }else{
            bucket[arr1[i]] = 1;
        }
    }
    for(let j = 0;j < arr2.length;j++){
        while(bucket[arr2[j]]){
            result.push(arr2[j]);
            bucket[arr2[j]]--;
        }
    }
    for(let key in bucket){
        while(bucket[key]){
            result.push(key);
            bucket[key]--;
        }
    }
    return result;
};
```