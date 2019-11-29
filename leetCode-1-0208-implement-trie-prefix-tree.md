# 实现 Trie (前缀树)（中等）
# 题目描述
![截屏2019-11-19下午7.28.12.png](https://pic.leetcode-cn.com/47cbb4f8604137c166b50cdc0b230a38f1e6d6d07f472eeda5a36dc29053f399-%E6%88%AA%E5%B1%8F2019-11-19%E4%B8%8B%E5%8D%887.28.12.png)
# 题目地址
<https://leetcode-cn.com/problems/implement-trie-prefix-tree/>
#### 解法一：26叉树标准解法
+ Trie，又叫前缀树或字典树，是一种有序树
+ 基本性质
  + 根节点不包含字符，除根节点以外每个节点只包含一个字符
  + 从根节点到某一个节点，路径上经过的字符连接起来，为该节点对应的字符串
  + 每个节点的所有子节点包含的字符串不相同
+ 此题的英文字母的字典树是一个26叉树
```javascript
// 字典树节点
class TreeNode{
    constructor(){
        // 是不是最后一个节点
        this.END = false;
        // 所有的儿子节点
        this.links = new Array(26);  
    }
    containsKey(letter) {
        return this.links[letter.charCodeAt()-97] != undefined;
    }
    getCh(letter){
        return this.links[letter.charCodeAt()-97];
    }
    putCh(letter,newTrieNode){
        this.links[letter.charCodeAt()-97] = newTrieNode;
    }
    setEnd(){
        this.END = true;
    }
    isEnd(){
        return this.END;
    }
}
let root = null;
/**
 * Initialize your data structure here.
 */
// 初始化字典树
var Trie = function() {
    root = new TreeNode();
};

/**
 * Inserts a word into the trie. 
 * @param {string} word
 * @return {void}
 */
// 搜索单词字符链接是否存在于字典树组成的链接中
let searchPrefix = (word) => {
    // 从根节点开始
    let currNode = root;
    for(let i = 0;i < word.length;i++){
        // 当前单词当前字符存在字典树正常向下遍历的链接中
        if(currNode.containsKey(word[i])){
            // 存在则继续向下
            currNode = currNode.getCh(word[i]);
        }else{
            // 不存在
            return null;
        }
    }
    // 单词遍历完，均在字典树中，说明单词字符链接至少是字典树中的前缀，
    // 或者是一条完整的链接即单词存在
    return currNode;
}
// 建立字典树
// 在自字典树中插入一个单词
Trie.prototype.insert = function(word) {
    let currNode = root;
    for(let i = 0;i < word.length;i++){
        // 当前节点没有儿子节点，则创建当前节点作为新的父节点，
        // 并创建一个新的TreeNode子节点与其相连
        if(!currNode.containsKey(word[i])){
            currNode.putCh(word[i],new TreeNode());
        }
        // 当前节点存在，意即当前单词中的当前字符存在于字典树中，
        // 则继续搜索下一个链接，即子节点
        currNode = currNode.getCh(word[i]);
    }
    // 直到搜索完毕，当前节点标记为结束节点
    currNode.setEnd();
};

/**
 * Returns if the word is in the trie. 
 * @param {string} word
 * @return {boolean}
 */
// 在字典树中查找一个完全匹配的单词
Trie.prototype.search = function(word) {
    // 将查找单词字符链接函数定义成一个函数，后面搜索前缀的可以复用
    let currNode = searchPrefix(word);
    // 如果单词至少是前缀，且字典树中也到了当前一条叉的尾部，
    // 则说明是完整的单词存在于字典树，
    // 否则最多是前缀即下面的stratsWith搜索
    return currNode != null && currNode.isEnd();
};

/**
 * Returns if there is any word in the trie that starts with the given prefix. 
 * @param {string} prefix
 * @return {boolean}
 */
// 分析同search操作
Trie.prototype.startsWith = function(prefix) {
    let currNode = searchPrefix(prefix);
    return currNode != null;
};

/** 
 * Your Trie object will be instantiated and called as such:
 * var obj = new Trie()
 * obj.insert(word)
 * var param_2 = obj.search(word)
 * var param_3 = obj.startsWith(prefix)
 */
```
#### 解法二：简化优化版
+ 动态构建每个节点的子节点个数，不一开始初始化26个
+ 把定义在class的方法糅合进执行函数里面，都是一样的
```javascript
class TrieNode{
    constructor(){
        this.END = false;
        this.children = {};
    }
}
let root = null;
/**
 * Initialize your data structure here.
 */
var Trie = function() {
    root  = new TrieNode();
};

/**
 * Inserts a word into the trie. 
 * @param {string} word
 * @return {void}
 */
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
/**
 * Returns if the word is in the trie. 
 * @param {string} word
 * @return {boolean}
 */
Trie.prototype.search = function(word) {
    let currNode = searchPrefix(word);
    return currNode != null && currNode.END;
};

/**
 * Returns if there is any word in the trie that starts with the given prefix. 
 * @param {string} prefix
 * @return {boolean}
 */
Trie.prototype.startsWith = function(prefix) {
    return searchPrefix(prefix) != null;
};

/** 
 * Your Trie object will be instantiated and called as such:
 * var obj = new Trie()
 * obj.insert(word)
 * var param_2 = obj.search(word)
 * var param_3 = obj.startsWith(prefix)
 */
```