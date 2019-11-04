# 字母异位词分组（中等）
# 题目描述
![截屏2019-10-21下午4.04.09.png](https://pic.leetcode-cn.com/457ab62c8a1820dd23a7b066c70aa480c5a04c1010bd89a19f35b2840d130fa3-%E6%88%AA%E5%B1%8F2019-10-21%E4%B8%8B%E5%8D%884.04.09.png)
# 题目地址
<https://leetcode-cn.com/problems/group-anagrams/solution/>
#### 解法一：暴力排序 sort
+ [242题的变种-戳看解法一](https://leetcode-cn.com/problems/valid-anagram/solution/242-you-xiao-de-zi-mu-yi-wei-ci-by-alexer-660/)
```javascript
/**
 * @param {string[]} strs
 * @return {string[][]}
 */
var groupAnagrams = function(strs) {
    var tmpCode = '';
    var hashMap = {};
    var result = [];
    for(var i = 0;i<strs.length;i++){
        tmpCode = strs[i].split('').sort().join('');
        if(hashMap[tmpCode]==undefined){
            hashMap[tmpCode] = [];
        }
        hashMap[tmpCode].push(strs[i]);
    }
    for(var key in hashMap){
        result.push(hashMap[key]);
    }
    return result;
};
```
#### 解法二：哈希表
+ [242题的变种-戳看解法二](https://leetcode-cn.com/problems/valid-anagram/solution/242-you-xiao-de-zi-mu-yi-wei-ci-by-alexer-660/)
+ 给每个字符做哈希映射
+ 遍历时，将每个字符hash值相同的归到同一组里去
```javascript
/**
 * @param {string[]} strs
 * @return {string[][]}
 */
var groupAnagrams = function(strs) {
    var tmpHash = new Array(26);
    var resultHash = {};
    var out = [];
    for(var i = 0;i<strs.length;i++){
        for(var k = 0;k<26;k++){
            tmpHash[k] = '0';
        }
        var aCode = 'a'.charCodeAt();
        var tmpI = strs[i];
        var len = tmpI.length;
        var resultHashKey = '';
        for(var r = 0;r<len;r++){
            tmpHash[tmpI[r].charCodeAt() - aCode]++;
        }
        var tmpHashKey = tmpHash.join('');
        if(resultHash[tmpHashKey]==undefined){
            resultHash[tmpHashKey] = [];
        }
        resultHash[tmpHashKey].push(tmpI)
    }
    
    for(var key in resultHash){
        out.push(resultHash[key])
    }
    return out;
};
```