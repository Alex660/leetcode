# 最长公共前缀（简单）
# 题目描述
![截屏2019-12-05下午3.19.47.png](https://pic.leetcode-cn.com/3f8a262a2bfebbf20603d6dc6c32494fffb87385da3e51121a95723deb3d2042-%E6%88%AA%E5%B1%8F2019-12-05%E4%B8%8B%E5%8D%883.19.47.png)
# 题目地址
<https://leetcode-cn.com/problems/longest-common-prefix/>
#### 解法一：行列遍历
+ 时间复杂度：O(S)，S 是所有字符串中字符数量的总和
+ 空间复杂度：O(1)
+ 从左往右遍历所有字符串
+ 以第一个字符串为模串对比
  + 比较每个字符串里的每个字符（不同字符串相同下标的字符）
    + 相同，遍历下一个字符
      + 其中一个字符串遍历到里结尾，意味着已经到达里最大的公共前缀
      + 类似最大公约数
    + 不同
      + 说明模串0到当前遍历索引i之间的字符是公共前缀，直接截取即可
+ 如果能遍历到结束
  + 说明模式串就是最大公共前缀
```javascript
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs) {
    if(!strs || strs.length == 0){
        return '';
    }
    let firstStr = strs[0];
    for(let i = 0;i < firstStr.length;i++){
        let prev = firstStr[i];
        for(let j = 1;j < strs.length;j++){
            if(i == strs[j].length || strs[j][i] != prev){
                return firstStr.substring(0,i);
            }
        }
    }
    return firstStr;
};
```
#### 解法二：逐位比较 + 拼接
+ 同样以第一个字符串为模式串，并且从左到右遍历模串的每个字符
  + 依次和剩下的字符串中的每个相同下标的字符做比较
    + 遇到相同索引位置不同字符时，说明最长公共前缀到此为止
    + 否则，每次拼接当前字符到结果中
+ 最后返回
```javascript
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs) {
    if(!strs || strs.length == 0){
        return '';
    }
    let res = '';
    for(let i = 0;i < strs[0].length;i++){
        for(let j = 1;j < strs.length;j++){
            if(strs[j][i] != strs[0][i]){
                return res;
            }
        }
        res += strs[0][i];
    }
    return res;
};
```
#### 解法三：排序 + 集合
+ sort将字符串数组按照编码排序：a-b-c-d-...-z
  + 则只需校验首、尾字符串的不同
  + 例如['ab','abc','ac','acd']
    + 直接判断首字符串是否包含于尾字符串
      + first == end
      + first + x == end
    + 否则
      + 逐位比较首、尾字符串的每个字符
```javascript
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs) {
    if(!strs || strs.length == 0){
        return '';
    }
    strs.sort();
    let first = strs[0];
    let end = strs[strs.length-1];
    // 此处 end.match(eval('/^'+first+'/')) 可以换成
    // end.indexOf(first) == 0，是一样的
    if(first == end || end.match(eval('/^'+first+'/'))){
        return first;
    }
    for(let i = 0;i < first.length;i++){
        if(first[i] != end[i]){
            return first.substring(0,i);
        }
    }
};
```
#### 解法四：库函数 + 水平扫描
+ 时间复杂度：O(S)，S 是所有字符串中字符数量的总和
+ 空间复杂度：O(1)
+ 依旧以首字符串为模式串
  + 依次遍历剩余的每个字符串是否包含首字符串
    + 如果不包含[此题解的包含，是从首字符开始算起]，则将模式串尾部字符去掉，重新外部循环
      + 例如：'abc'.indexOf('bc') != 0
      + 如果模式串为空，说明整个字符串数组没有公共前缀，返回空
    + 如果包含，继续下一个字符串判断是否包含
      + 例如：'abc'.indexOf('ab') == 0
  + 遍历都结束后，剩下的被一一截取掉不合题意的尾部字符，所剩下的模式串，即为所求
```javascript
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs) {
    if(!strs || strs.length == 0){
        return '';
    }
    let prev = strs[0];
    for(let i = 0;i < strs.length;i++){
        while(strs[i].indexOf(prev) != 0){
            prev = prev.substring(0,prev.length-1);
            if(!prev){
                return '';
            }
        }
    }
    return prev;
};
```
#### 解法五：Trie 树
+ 时间复杂度：
  + 预处理过程 O(S)，其中 SS 数组里所有字符串中字符数量的总和，
  + 最长公共前缀查询操作的复杂度为 O(m)，
  + 建立字典树的时间复杂度为 O(S)，
  + 在字典树中查找字符串 q 的最长公共前缀在最坏情况下需要 O(m)的时间。
+ 空间复杂度：
  + O(S)，使用额外的 SS 空间建立字典树。
+ 先来了解Trie树
  + [208. 实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/solution/208-shi-xian-trie-qian-zhui-shu-by-alexer-660/)
+ 通过 Trie 树，可以很容易判断字符串数组中是否没有同一个公共前缀
  + 如果其中一个有前缀
    + 且当前节点Trie树中不分叉，且不是最后一个节点，说明此前缀是公共前缀
```javascript
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs) {
    if(!strs || strs.length == 0){
        return '';
    }
    class TrieNode{
        constructor(){
            this.END = false;
            this.children = {};
        }
    }
    let root = null;
    var Trie = function() {
        root  = new TrieNode();
    };
    Trie.prototype.insert = function(word) {
        let currNode = root;
        for(let i = 0;i < word.length;i++){
            if(currNode.children[word[i]] == undefined){
                currNode.children[word[i]] = new TrieNode();
            }
            currNode = currNode.children[word[i]];
        }
        currNode.END = true;
    };
    let searchPrefix = (word) => {
        let currNode = root;
        for(let i = 0;i < word.length;i++){
            if(currNode.children[word[i]]){
                currNode = currNode.children[word[i]];
            }else{
                return null;
            }
        }
        return currNode;
    }
    Trie.prototype.startsWith = function(prefix) {
        return searchPrefix(prefix) != null;
    };
    Trie.prototype.findNode = function(prefix) {
        return searchPrefix(prefix);
    };
    let strsTrie = new Trie();
    for(let i = 0;i < strs.length;i++){
        strsTrie.insert(strs[i]);
    }
    let prev = strs[0];
    let res = '';
    let node = root;
    for(let j = 0;j < prev.length;j++){
        let currLetter = prev[j];
        if(strsTrie.startsWith(res + currLetter) && !node.END && Object.values(node.children).length == 1){
            node = strsTrie.findNode(res + currLetter);
            res += currLetter;
        }else{
            return res;            
        }
    }
    return res;
};
```