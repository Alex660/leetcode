# 单词接龙（中等）
# 题目描述
![截屏2019-11-07上午5.13.56.png](https://pic.leetcode-cn.com/51ae018b04c5aa3525d3e160de2b683494bb192ce7667643932b0c987ce684ff-%E6%88%AA%E5%B1%8F2019-11-07%E4%B8%8A%E5%8D%885.13.56.png)
![截屏2019-11-07上午5.14.07.png](https://pic.leetcode-cn.com/88d8f46a8f86686da7eaa8b0ffccbf252163d1bca74a93e9c1476d1f153eba08-%E6%88%AA%E5%B1%8F2019-11-07%E4%B8%8A%E5%8D%885.14.07.png)
# 题目地址
<https://leetcode-cn.com/problems/word-ladder/description/>
## 解法七：
![截屏2019-11-23下午11.25.29.png](https://pic.leetcode-cn.com/8f7dc0d167d7aefff0dfc84a6682de19e10c42f6a7e4702c86af202d78bd0bf3-%E6%88%AA%E5%B1%8F2019-11-23%E4%B8%8B%E5%8D%8811.25.29.png)
## 解法八：
![截屏2019-11-23下午11.47.38.png](https://pic.leetcode-cn.com/fc08d2935a8464ba1ba4b74778a078f479baf5a77d13abc97b2e736162daf93c-%E6%88%AA%E5%B1%8F2019-11-23%E4%B8%8B%E5%8D%8811.47.38.png)
#### 解法一：BFS
+ 时间复杂度：O(M * N) ,M为单词长度，,N为单词列表长度
+ 空间复杂度：O(M * N) ,M长度的单词化为邻接单词形式时需要M，N同上
+ 用广度优先搜索搜索从beginWord到endWord的最短路径问题
+ 思路分析
  >一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog", 
  + 从起点"hit"变换到终点"cog"
    + 每次变换
      + 只能变动其中一个单词
      + 且必须在字典中
  + 因此每次变换，其实就是走了一步
  + 每次变换前后的单词差异只有其中一个字符   
    + 设唯一不同的字符为*
    > "hit" -> "hot" -> "dot" -> "dog" -> "cog"
      + hit ---->  h*t  <-----hot
      + dot ---->  *ot  <-----hot 
      + dot ---->  do*  <-----dog
      + dog ---->  *og  <-----cog
    + 我们把相差唯一一个字符的两个单词称为邻接单词
      + 因此 单词'xyz'的所有邻接单词为
        + '*yz'   
        + 'x*z'
        + 'xy*'
  + 因为单词的下一个变换单词即是邻接单词
    + 例如我们要从'hit'变换到'hot'
      + hit的邻接单词形式有
        + *it
        + h*t
        + hi*
      + hot的邻接单词形式有
        + *ot
        + h*t
        + ho*   
      + 我们寻找他们相同的邻接单词形式
        + 为h*t
      + 反过来
        + 符合h*t形式的单词有 
          + hit
          + hot
        + 因此在代码中，此处便是寻找下一邻接单词的关键逻辑
+ 代码逻辑
  1. 求出wordList中的所有单词的所有邻接单词形式
    1. 将具有相同邻接单词形式的单词放在一个集合里
       1. 形如 hash[邻接单词形式] = [单词1,单词2,...,单词n]  
    2. 当我们在寻找下一个邻接单词是谁时，我们通过求当前单词的邻接单词的形式，去枚举上一步的所有邻接单词形式的集合
       1. 存在则在邻接单词形式对应的单词集合里，遍历寻找符合的目标单词
          1. 找到，则返回
          2. 没找到，当前寻址步数++，继续寻址
  2. 我们将从起点 beginWord --> endWord 的寻址路线，抽象为队列
     1. 初始值是起点，入队
     2. 出队，通过邻接单词形式变换寻址，逻辑如上 
  3. 核心逻辑在第一步
  4. 解题技巧
     1. 每个邻接单词形式对应的单词既有可能相同，会出现循环寻址即环的形式
        1. 例如 h*t -> hot、*ot -> hot
        2. 因此维护一个访问记录数组，访问比较过的单词，下次不再访问
```javascript
/**
 * @param {string} beginWord
 * @param {string} endWord
 * @param {string[]} wordList
 * @return {number}
 */
var ladderLength = function(beginWord, endWord, wordList) {
    if(!endWord || wordList.indexOf(endWord) == -1){
        return 0;
    }
	// 各个通用状态对应所有单词
	var comboDicts = {};
	var len = beginWord.length;
    for(var i = 0;i<wordList.length;i++){
		for(var r = 0;r<len;r++){
			var newWord = wordList[i].substring(0,r)+'*'+wordList[i].substring(r+1,len);
			(!comboDicts[newWord]) && (comboDicts[newWord] = []);
			comboDicts[newWord].push(wordList[i]);
		}
	}
	// Queue for BFS
	var queue = [[beginWord,1]];
	// visited
	var visited = {beginWord:true};
	while(queue.length > 0){
		var currNode = queue.shift();
		var currWord = currNode[0];
		var currLevel = currNode[1];
		for(var i = 0;i < len;i++){
            // 通用状态
			var newWord = currWord.substring(0,i)+'*'+currWord.substring(i+1,len);
            if(newWord in comboDicts){
                var tmpWords = comboDicts[newWord];
                for(var z = 0;z<tmpWords.length;z++){
                    if(tmpWords[z] == endWord){
                        return currLevel + 1;
                    }
                    if(!visited[tmpWords[z]]){
                        visited[tmpWords[z]] = true;
                        queue.push([tmpWords[z],currLevel+1]);
                    }
                }
            }
		}
	}
	return 0;
};
```
#### 解法二：DFS
+ [思路同433. 最小基因变化-解法三](https://leetcode-cn.com/problems/minimum-genetic-mutation/solution/433-zui-xiao-ji-yin-bian-hua-by-alexer-660/)
+ 这个方法会超时
```javascript
/**
 * @param {string} beginWord
 * @param {string} endWord
 * @param {string[]} wordList
 * @return {number}
 */
var ladderLength = function(beginWord, endWord, wordList) {
    if(!endWord || wordList.indexOf(endWord) == -1){
        return 0;
    }
    var visited = {};
    var minLevel = Number.MAX_SAFE_INTEGER;
    var level = 1;
    function recurse(beginWord,level){
        if(beginWord == endWord){
            minLevel = Math.min(minLevel,level);
        }
        for(var i = 0;i<wordList.length;i++){
            var tmpWord = wordList[i];
            var diff = 0;
            for(var r = 0;r<tmpWord.length;r++){
                if(beginWord[r] != tmpWord[r]){
                    diff++;
                    if(diff > 1){
                        break;
                    }
                }
            }
            if(diff == 1 && !visited[tmpWord]){
                visited[tmpWord] = true;
                recurse(tmpWord,level+1);
                visited[tmpWord] = false;
            }
        }
    }
    recurse(beginWord,level);
    return (minLevel ^ Number.MAX_SAFE_INTEGER) == 0 ? 0 : minLevel;
};
```
#### 解法三：双端BFS
+ 时间复杂度：O(M*N)
+ 空间复杂度：O(M*N)
+ 核心思想是解法一
  + 类似双指针的思想
    + 同时从头尾两个部分搜索遍历
    + 搜索终止条件为
      +  一个节点被两个人搜索过
      +  类似相遇问题
```javascript
/**
 * @param {string} beginWord
 * @param {string} endWord
 * @param {string[]} wordList
 * @return {number}
 */
var ladderLength = function(beginWord, endWord, wordList) {
    if(!endWord || wordList.indexOf(endWord) == -1){
        return 0;
    }
	// 各个通用状态对应所有单词
	var comboDicts = {};
	var len = beginWord.length;
    for(var i = 0;i<wordList.length;i++){
		for(var r = 0;r<len;r++){
			var newWord = wordList[i].substring(0,r)+'*'+wordList[i].substring(r+1,len);
			(!comboDicts[newWord]) && (comboDicts[newWord] = []);
			comboDicts[newWord].push(wordList[i]);
		}
	}
    
    function visitWord(currQueue,currVisited,othersVisited){
        var currNode = currQueue.shift();
		var currWord = currNode[0];
		var currLevel = currNode[1];
		for(var i = 0;i < len;i++){
            // 通用状态
			var newWord = currWord.substring(0,i)+'*'+currWord.substring(i+1,len);
            if(newWord in comboDicts){
                var tmpWords = comboDicts[newWord];
                for(var z = 0;z<tmpWords.length;z++){
                    if(othersVisited[tmpWords[z]] != undefined){
                        return currLevel + othersVisited[tmpWords[z]];
                    }
                    if(currVisited[tmpWords[z]] == undefined){
                        currVisited[tmpWords[z]] = currLevel + 1;
                        currQueue.push([tmpWords[z],currLevel+1]);
                    }
                }
            }
		}
        return -1;
    }
    
	// Queue for BFS from beginWord
	var queueBegin = [[beginWord,1]];
    // Queue for BFS from endWord
    var queueEnd  = [[endWord,1]];
	// visited begin and end
	var visitedBegin = {};
    visitedBegin[beginWord] = 1;
	var visitedEnd = {};
    visitedEnd[endWord] = 1;
	while(queueBegin.length > 0 && queueEnd.length > 0){
        // fromBegin
        var ans = visitWord(queueBegin,visitedBegin,visitedEnd);
        if(ans > -1){
            return ans;
        }
        // formEnd
        ans = visitWord(queueEnd,visitedEnd,visitedBegin);
        if(ans > -1){
            return ans;
        }
	}
	return 0;
};
```
#### 解法四：BFS-2
+ [思路同433. 最小基因变化-解法一](https://leetcode-cn.com/problems/minimum-genetic-mutation/solution/433-zui-xiao-ji-yin-bian-hua-by-alexer-660/)
```javascript
/**
 * @param {string} beginWord
 * @param {string} endWord
 * @param {string[]} wordList
 * @return {number}
 */
var ladderLength = function(beginWord, endWord, wordList) {
    if(!endWord || wordList.indexOf(endWord) == -1){
        return 0;
    }
    var wordListHash = {};
    for(var i = 0;i<wordList.length;i++){
        wordListHash[wordList[i]] = true;
    }
    var genes = 'abcdefghigklmnopqrstuvwxyz';
    var level = 0;
    var queue = [[beginWord,1]];
    while(queue.length != 0){
        var curr = queue.pop();
        level = curr[1]
        if(curr[0] == endWord){
            return level;
        }
        var arrCurr = curr[0];
        for(var i = 0;i<arrCurr.length;i++){
            for(var r = 0;r<genes.length;r++){
                if(genes[r] != arrCurr[i]){
                    var strCurr = (arrCurr.slice(0,i))+genes[r]+(arrCurr.slice(i+1));
                    if(wordListHash[strCurr]){
                        queue.unshift([strCurr,level+1]);
                        wordListHash[strCurr] = false;
                    }
                }
            }
        }
    }
	return 0;
};
```
+ 或者这样写也可
```javascript
/**
* @param {string} beginWord
* @param {string} endWord
* @param {string[]} wordList
* @return {number}
*/
var ladderLength = function(beginWord, endWord, wordList) {
    let comboDicts = new Set(wordList);
    if(!comboDicts.has(endWord)){
        return 0;
    }
    let level = 0;
    let queue = [[beginWord,1]];
    while(queue.length != 0){
        let currNode = queue.pop();
        let currLeter = currNode[0];
        level = currNode[1];
        if(currNode[0] == endWord){
            return level;
        }
        for(let i = 0;i < currLeter.length;i++){
            for(r = 0;r < 26;r++){
                let genTmp = String.fromCharCode(97+r);
                if( genTmp != currLeter[i]){
                    let str = currLeter.slice(0,i)+genTmp+currLeter.slice(i+1);
                    if(comboDicts.has(str)){
                        queue.unshift([str,level+1]);
                        comboDicts.delete(str);
                    }
                }
            }
        }
    }
    return 0;
}; 
``` 
#### 解法五：双端BFS升级版
+ 此解法时间复杂度比以上更低
  + 解法一和解法二共同的核心思想 
    + 用邻接单词形式【*yz、x*z、xy*】作为键
    + 用符合邻接单词形式的单词作为值
      + 寻址共同的邻接单词形式为前提，遍历对应的值
        + 找到返回前一步步数加1
        + 没找到继续遍历
  + 此解法的核心思想
    + 借用了[433. 最小基因变化-解法一](https://leetcode-cn.com/problems/minimum-genetic-mutation/solution/433-zui-xiao-ji-yin-bian-hua-by-alexer-660/) 的思路
      + 通过直接重新组合上一步单词
        + 下一步变换为可能且符合条件的单词，在其中遍历寻找
          + 找到返回...
          + 没找到步数++，继续遍历
      + 单端亦可以用此思路
      + 双端则需要加上本题解法三的思路
+ 合并两道题解的思路即为如下解法 
```javascript
/**
 * @param {string} beginWord
 * @param {string} endWord
 * @param {string[]} wordList
 * @return {number}
 */
var ladderLength = function(beginWord, endWord, wordList) {
	var comboDicts = new Set(wordList);
    if(!comboDicts.has(endWord)){
        return 0;
    }
	var len = beginWord.length;
    
    function gens(word){
        var result = [];
        var len = word.length;
        for(var i = 0;i<len;i++){
            for(var j = 0;j < 26;j++){
                var newWord = word.substring(0,i)+String.fromCharCode(j+97)+word.substring(i+1);
                if(comboDicts.has(newWord) && newWord != word){
                    result.push(newWord);
                }
            }
        }
        return result;
    }
    
    function visitWord(currQueue,currVisited,othersVisited){
        var currNode = currQueue.shift();
		var tmpWords = gens(currNode[0]);
        var currLevel = currNode[1];
        var tmpLen = tmpWords.length;
        for(var i = 0;i< tmpLen;i++){
            if(othersVisited[tmpWords[i]] != undefined){
                return currLevel + othersVisited[tmpWords[i]];
            }
            if(currVisited[tmpWords[i]] == undefined){
                currVisited[tmpWords[i]] = currLevel + 1;
                currQueue.push([tmpWords[i],currLevel + 1])
            }
        } 
        return -1;
    }
    
	// Queue for BFS from beginWord
	var queueBegin = [[beginWord,1]];
    // Queue for BFS from endWord
    var queueEnd  = [[endWord,1]];
	// visited begin and end
	var visitedBegin = {};
    visitedBegin[beginWord] = 1;
	var visitedEnd = {};
    visitedEnd[endWord] = 1;
	while(queueBegin.length > 0 && queueEnd.length > 0){
        // fromBegin
        var ans = visitWord(queueBegin,visitedBegin,visitedEnd);
        if(ans > -1){
            return ans;
        }
        // formEnd
        ans = visitWord(queueEnd,visitedEnd,visitedBegin);
        if(ans > -1){
            return ans;
        }
	}
	return 0;
};
```
#### 解法六：双端BFS强化版
+ 其实是**解法四：BFS-2 第二种写法的另类版**
  + 将queue换成set
+ Set 对象搜索、删除、插入时间复杂度为O(1)
+ beginSet、endSet各自向一边扩散
+ visited：节点是否已经访问过
+ BFS
  + beginSet、endSet相当于两个queue
  + 扩散过程中，优先选择小的set
    + 交换set
  + 因上面作了交换，每次只需从beginSet开始扩散即可
  + 定义一个temp Set 
    + 相当于向queue不断加入扩散中被选中的元素
    + 因此一轮遍历完，直接将beginSet替换为temp即可
      + 每次遍历到一个单词
        + 遍历单词的每个字符，并尝试替换为与当前不相同的其余25个字符
          + 如果从尾部扩散出的endSet有了新组合的单词
            + 说明首尾相遇，直接返回当前level+1即可
          + 否则，如果新组合单词未在访问备忘录visited中出现，且出现在单词库wordList中时
            + 加入tempSet，用于下一轮扩散单词遍历组合操作
            + 并且加入备忘录visited
  + 继续下一轮queue temp set遍历
```javascript
/**
 * @param {string} beginWord
 * @param {string} endWord
 * @param {string[]} wordList
 * @return {number}
 */
let ladderLength = function(beginWord, endWord, wordList) {
    let wordListSet = new Set(wordList);
    if(!wordListSet.has(endWord)){
        return 0;
    }
    let beginSet = new Set();
    beginSet.add(beginWord);
    let endSet = new Set();
    endSet.add(endWord)
    let visited = new Set();
    let level = 1;
    // BFS
    while (beginSet.size > 0 && endSet.size > 0) {
        if(beginSet.size > endSet.size){
            let tmp = beginSet;
            beginSet = endSet;
            endSet = tmp;
        }
        let temp = new Set();
        for(let key of beginSet){
            for(let i = 0;i < key.length;i++){
                for(let j = 0;j < 26;j++){
                   let tmp = key.slice(0,i)+String.fromCharCode(97+j)+key.slice(i+1);
                   if(endSet.has(tmp)){
                       return level + 1;
                   }
                   if(!visited.has(tmp) && wordListSet.has(tmp)){
                        temp.add(tmp);
                        visited.add(tmp);
                   }
                }
            }
        }
        beginSet = temp;
        level++;
    }
    return 0;
}
```
#### 解法七：双端BFS终极版
+ 将解法六的visited去掉，直接操作原数组
+ 并对部分变量命名作了语义化处理，更容易理解
+ **此解法是所有解法中最快的**
```javascript
/**
 * @param {string} beginWord
 * @param {string} endWord
 * @param {string[]} wordList
 * @return {number}
 */
let ladderLength = function(beginWord, endWord, wordList) {
    let wordListSet = new Set(wordList);
    if(!wordListSet.has(endWord)){
        return 0;
    }
    let beginSet = new Set();
    beginSet.add(beginWord);
    let endSet = new Set();
    endSet.add(endWord)
    let level = 1;
    // BFS
    while (beginSet.size > 0) {
        let next_beginSet = new Set();
        for(let key of beginSet){
            for(let i = 0;i < key.length;i++){
                for(let j = 0;j < 26;j++){
                   let s =  String.fromCharCode(97+j);
                   if(s != key[i]){
                        let new_word = key.slice(0,i)+s+key.slice(i+1);
                        if(endSet.has(new_word)){
                            return level + 1;
                        }
                        if(wordListSet.has(new_word)){
                            next_beginSet.add(new_word);
                            wordListSet.delete(new_word);
                        }
                   }
                }
            }
        }
        beginSet = next_beginSet;
        level++;
        if(beginSet.size > endSet.size){
            let tmp = beginSet;
            beginSet = endSet;
            endSet = tmp;
        }
    }
    return 0;
}
```
#### 解法八：双端BFS灭世版
+ 解法七的交换操作去掉中间变量缓存，直接用ES6的交换语法糖
+ 所幸性能直接飙升，击败百分百。
+ ![截屏2019-11-23下午11.47.38.png](https://pic.leetcode-cn.com/fc08d2935a8464ba1ba4b74778a078f479baf5a77d13abc97b2e736162daf93c-%E6%88%AA%E5%B1%8F2019-11-23%E4%B8%8B%E5%8D%8811.47.38.png)
```javascript
/**
 * @param {string} beginWord
 * @param {string} endWord
 * @param {string[]} wordList
 * @return {number}
 */
let ladderLength = function(beginWord, endWord, wordList) {
    let wordListSet = new Set(wordList);
    if(!wordListSet.has(endWord)){
        return 0;
    }
    let beginSet = new Set();
    beginSet.add(beginWord);
    let endSet = new Set();
    endSet.add(endWord)
    let level = 1;
    // BFS
    while (beginSet.size > 0) {
        let next_beginSet = new Set();
        for(let key of beginSet){
            for(let i = 0;i < key.length;i++){
                for(let j = 0;j < 26;j++){
                   let s =  String.fromCharCode(97+j);
                   if(s != key[i]){
                        let new_word = key.slice(0,i)+s+key.slice(i+1);
                        if(endSet.has(new_word)){
                            return level + 1;
                        }
                        if(wordListSet.has(new_word)){
                            next_beginSet.add(new_word);
                            wordListSet.delete(new_word);
                        }
                   }
                }
            }
        }
        beginSet = next_beginSet;
        level++;
        if(beginSet.size > endSet.size){
            [beginSet,endSet] = [endSet,beginSet]
        }
    }
    return 0;
}
```