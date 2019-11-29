# 有效的字母异位词（简单）
# 题目描述
![截屏2019-10-21下午3.00.22.png](https://pic.leetcode-cn.com/2ee65af6073864bd7295b324f466f2c87dac54cd926b427f55ffb577ba7b1e6c-%E6%88%AA%E5%B1%8F2019-10-21%E4%B8%8B%E5%8D%883.00.22.png)
# 题目地址
<https://leetcode-cn.com/problems/valid-anagram/>
### 分析题意
+ 一边单词字母出现的频率等于另一边单词出现的频率但字母位置可以不一样
+ 两边单词个数不同肯定为 false
#### 解法一：暴力排序 sort O(NlogN)
+ 两边字母转换为数组再排序拼接判断两边值是否相等即可
```javascript
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var isAnagram = function(s, t) {
    if(s.length != t.length){
        return false;
    }
    var sSort = s.split('').sort();
    var tSort = t.split('').sort();
    return sSort.join('') == tSort.join('');
};
```
#### 解法二：计数排序 + 哈希表
+ 类似题型
  + [1122. 数组的相对排序](https://leetcode-cn.com/problems/relative-sort-array/solution/1122-shu-zu-de-xiang-dui-pai-xu-by-alexer-660/)
+ 时间复杂度 O(n)
+ 空间复杂度 O(1)  O(26)==O(1) 表大小不变复杂性不变
+ 分别对其中一个单词每个字母出现的字符次数进行增加
+ 对另一给单词每个字母出现的次数进行减少
+ 维护一个26位的数组，且初始化为0，方便遍历时直接进行++，而不用判断是否存在
+ 最后遍历哈希结果数组只要有不为0的就是false
```javascript
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var isAnagram = function(s, t) {
    if(s.length != t.length){
        return false;
    }
    var result = new Array(26);
    for(var i = 0;i<26;i++){
        result[i] = 0;
    }
    var aCode = 'a'.charCodeAt();
    for(var i = 0;i<s.length;i++){
        result[s[i].charCodeAt()-aCode]++;
        result[t[i].charCodeAt()-aCode]--;
    }
    for(var r = 0;r<result.length;r++){
        if(result[r]!=0){
            return false;
        }
    }
    return true;
};
```
+ 优化版
+ 放在第二次遍历做判断，第一次遍历只维护一个增加表
+ 第二次只维护一个减小表，只要小于0就返回false
```javascript
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var isAnagram = function(s, t) {
    if(s.length != t.length){
        return false;
    }
    var result = new Array(26);
    for(var i = 0;i<26;i++){
        result[i] = 0;
    }
    var aCode = 'a'.charCodeAt();
    for(var i = 0;i<s.length;i++){
        result[s[i].charCodeAt()-aCode]++;
    }
    for(var r = 0;r<t.length;r++){
        var tmpCode = t[r].charCodeAt()-aCode;
        result[tmpCode]--;
        if(result[tmpCode] < 0){
           return false;
        }
    }
    return true;
};
```