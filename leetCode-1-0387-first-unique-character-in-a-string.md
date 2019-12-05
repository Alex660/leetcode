# 字符串中的第一个唯一字符（简单）
# 题目描述
![截屏2019-12-05上午10.27.45.png](https://pic.leetcode-cn.com/cafd94f368c58ac7aeeb0051cc4be6b03c08f60664b3db7db0612dfed8b159f0-%E6%88%AA%E5%B1%8F2019-12-05%E4%B8%8A%E5%8D%8810.27.45.png)
# 题目地址
<https://leetcode-cn.com/problems/first-unique-character-in-a-string/>
#### 解法一：哈希 + Map
+ 哈希
  + 唯一性、查询O(1)
+ Map
  + 按插入顺序存储数据
+ 从左到右遍历
  + 第一次存在于哈希表，则存进去，并插入到map中
  + 非首次，从map中删除，说明当前字符不属于结果中
  + 最后返回map中第一个值
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var firstUniqChar = function(s) {
   let hash = {};
   let result = new Map(); 
   for(let i = 0;i < s.length;i++){
       if(!hash[s[i]]){
           hash[s[i]] = 1;
           result.set(s[i],i);
       }else{
           result.delete(s[i]);           
       }
   }
   if(result.size == 0){
       return -1;
   }
   return result.values().next().value;
};
```
#### 解法二：哈希 + 遍历
+ 与上面唯一不同的是，这里最后是通过遍历来查询第一个符合的结果
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var firstUniqChar = function(s) {
   let hash = {};
   let result = [];
   for(let i = 0;i < s.length;i++){
       if(!hash[s[i]]){
           hash[s[i]] = 1;
       }else{
           hash[s[i]]++;
       }
   }
   for(let j = 0;j < s.length;j++){
       if(hash[s[j]] == 1){
           return j;
       }
   }
    return -1;
};
```
#### 解法三：库函数
+ 从首字符开始寻找和从末尾开始寻找得到的相同字符的两个索引如果一样，说明只有一个字符，且一定是第一个出现的
  + 虽然方便，但当前所有解法中，解法一是最快的
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var firstUniqChar = function(s) {
   for(let i = 0;i < s.length;i++){
       if(s.indexOf(s[i]) == s.lastIndexOf(s[i])){
           return i;
       }
   }
    return -1;
};
```
#### 解法四：计数排序
+ 是当前所有解法中，执行用时最少的
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var firstUniqChar = function(s) {
    let countingSort = (arr,maxValue) => {
        let bucket = new Array(maxValue).fill(0);
        let arrLen = arr.length;
        for(let i = 0;i < arrLen;i++){
            bucket[arr[i].charCodeAt() - 97]++;
        }
        for(let j = 0;j < arrLen;j++){
            if(bucket[arr[j].charCodeAt() - 97] == 1){
                return j;
            }
        }
        return -1;
    }
    return countingSort(s,26)
};
```
#### 解法五：遍历 + 比较
+ 题意只有小写字母
  + 因此最多只会出现26个字母中的一个
+ 遍历26个字母，原字符串中首尾搜索是同一个字符且是同一索引值，
  + 且索引最小的最靠前，也就是第一个只出现一个的字符字母
    + 通过比较最小值
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var firstUniqChar = function(s) {
    if(s.length === 1) {
        return 0;
    }
    let base = ['a', 'b', 'c', 'd', 'e', 'f', 'g',
        'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p',
        'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y',
        'z'
    ];
    let minIndex = Number.MAX_SAFE_INTEGER, firstIndex;
    for(let i = 0; i < base.length; i++) {
        firstIndex = s.indexOf(base[i]);
        if(firstIndex >=0 && firstIndex === s.lastIndexOf(base[i])) {
            minIndex = Math.min(minIndex, firstIndex);
        }
    }
    return (minIndex ^ Number.MAX_SAFE_INTEGER) == 0 ? -1 : minIndex;
};
```