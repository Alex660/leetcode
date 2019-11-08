# 最小基因变化（中等）
# 题目描述
![截屏2019-10-28下午3.24.20.png](https://pic.leetcode-cn.com/1ebe10a2b59878e70158ba9f34179e5bc22c13b10c9c175d2488164b399c65c6-%E6%88%AA%E5%B1%8F2019-10-28%E4%B8%8B%E5%8D%883.24.20.png)
![截屏2019-10-28下午3.24.35.png](https://pic.leetcode-cn.com/07cdcd43d33e83255d808d605adae1f057215166a1971b750806dd27a53a0350-%E6%88%AA%E5%B1%8F2019-10-28%E4%B8%8B%E5%8D%883.24.35.png)
# 题目地址
<https://leetcode-cn.com/problems/minimum-genetic-mutation/#/description>
#### 解法一：BFS-1
+ 广度优先遍历：从左到右
+ 用队列
+ 以下所有广度代码队列均是 左边入队、右边出队
+ 具体解析我就写在代码上了，好理解不是
```javascript
/**
 * @param {string} start
 * @param {string} end
 * @param {string[]} bank
 * @return {number}
 */
var minMutation = function(start, end, bank) {
    // 起始基因序列与目标序列相同 则不用变换 次数为0
    if(start == end){
        return 0;
    }
    // 创建基因组库，用于判断变换之后的起始基因串是否被包含在此库内，在才符合题意
    // Map hash模式相比数组啥的判断是否存在 性能较优
    var bankSet = new Map();
    for(var i = 0;i<bank.length;i++){
        bankSet.set(bank[i],true);
    }
    // 创建基因原始字符 即所有基因字符都来源于此
    // 用于将起始基因串中的单个字符遍历修改为基因原始字符重新组合 去广度搜索是否有符合题意的变化后的基因串
    // 且因为遍历组合有重复的状态可能因此在下方创建访问过的visited的数组
    var genes = ['A','C','G','T'];
    // 初始化变换次数
    var level = 0;
    // 原因在上
    var visited = new Map();
    // 起始基因串也不能重复
    visited.set(start,true);
    // 起始基因串作为队列的初始状态需要和原始基因字符组进行广度组合(从左到右依次更换)并判断在基因组库是否存在
    var queue = [start];
    while(queue.length != 0){
        // 每次size为每次经过和原始字符集合数组组合的所有可能个数
        // 题意即是求这组合的所有可能中有无目标串
            // 每一轮组合即为1步
                // 当其中一轮有curr == end 出现找到了匹配目标串的经过组合的起始基因串返回经过的记录的步数即为所求 
                // 当一次又一次经过所有轮组合的基因串入队出队直到队列为0 即再也没有可以经过返回组合的一组基因串时，说明找不到可以组合匹配目标串的起始基因串 返回-1
        var size = queue.length;
        // size为队列的数量，每一次入队则为符合要求的变换
        while(size > 0){
            // 出队
            var curr = queue.pop();
            //如果符合目标基因串 则找到了 返回 变换次数
            if(curr == end){
                return level;
            }
            var arrCurr = curr.split('');
            for(var i = 0;i<arrCurr.length;i++){
                var oldCurr = arrCurr[i];
                for(var r = 0;r<genes.length;r++){
                    arrCurr[i] = genes[r];
                    var strCurr = arrCurr.join('');
                    // 存在则为符合要求(每一次基因变化的结果都需要是一个合法的基因串)
                    if(!visited.has(strCurr) && bankSet.has(strCurr)){
                        visited.set(strCurr,true);
                        // 入队
                        queue.unshift(strCurr);
                    }
                    // 不存在则说明变换后的次数不符合 进行下一个原始基因字符组的替换重组(从左到右)
                }
                arrCurr[i] = oldCurr;
            }
            // 将上一轮经过变换组合后与基因库匹配但与目标基因串不匹配但所有队列数组重新匹配上一轮匹配出的仅仅匹配基因库的所有基因串的个数和
            // 如果直到匹配个数和次仍旧没有匹配到同样匹配目标串的基因串
                // 说明当前这一步这一轮不够要进行下一步下一轮的重新组合匹配
            size--;
        }
        // 每一轮变换 将得到的最近的一个变换后的基因串出队再进入下一轮变换    
        // 一轮为一次经过的所有基因组合但不匹配目标串的经过的步数
        level++;
        // 例子 size 和 level 的相互作用
        // 输入
        // "AAAACCCC" --- 起始基因串
        // "CCCCCCCC" --- 目标基因串
        // 基因组库
        // ["AAAACCCA","AAACCCCA","AACCCCCA","AACCCCCC","ACCCCCCC","CCCCCCCC","AAACCCCC","AACCCCCC"]
        // 原始基因字符组
        //['A','C','G','T']

        // 输出变换组合过程
        // size--: 0
        // [ 'AAAACCCA', 'AAACCCCC' ]

        // size--: 1
        // size--: 0
        // [ 'AAACCCCA', 'AACCCCCC' ]

        // size--: 1
        // size--: 0
        // [ 'AACCCCCA', 'ACCCCCCC' ]

        // 符合题意的最后一步最后一轮组合匹配成功
        // size--: 1
        // size--: 0
        // [ 'CCCCCCCC' ]
    }
    return -1;
};
```
+ 优化+1
  + 上面维护了一个visited 数组
    + 其实不必，因为如果在第一次比较后就从bankSet中删掉的话，下一此碰到同样组合的strCurr 因其在bankSet即基因库中已经不存在 所以同样可以达到规避重复遍历比较的效果 且 去掉了一个判断和多余的存储空间
  + 上面遍历组合原始字符组其中一个字符的时候 会碰到和变换前起始基因串相同的字符 遇到此情况时 可以直接跳过
    + 第一种是加上一层判断 即 
    > if(arrCurr[i] != genes[r]) 为真才进入内循环 对上面部分代码作出改动如下 其它不不变
      ```javascript
      for(var r = 0;r<genes.length;r++){
           // 准入条件
            if(arrCurr[i] != genes[r]){
                arrCurr[i] = genes[r];
                var strCurr = arrCurr.join('');
                if(bankSet.has(strCurr)){
                    bankSet.delete(strCurr);
                    queue.unshift(strCurr);
                }
            }
        }
      ```
    + 第二种直接对genes字符组进行改动如下，起始基因串从左到右遍历到其中一个字符时 可组合的字符序列一定不包括本身 再内循环取的原始字符组就直接不存在相同的字符了 改成代码如下
```javascript
/**
 * @param {string} start
 * @param {string} end
 * @param {string[]} bank
 * @return {number}
 */
var minMutation = function(start, end, bank) {
    if(start == end){
        return 0;
    }
    var bankSet = new Map();
    for(var i = 0;i<bank.length;i++){
        bankSet.set(bank[i],true);
    }
    var genes = {'A':["C","G","T"],'C':["A","G","T"],'G':["A","C","T"],'T':["A","C","G"]};
    var level = 0;
    var queue = [start];
    while(queue.length != 0){
        var size = queue.length;
        while(size > 0){
            var curr = queue.pop();
            if(curr == end){
                return level;
            }
            var arrCurr = curr.split('');
            for(var i = 0;i<arrCurr.length;i++){
                var oldCurr = arrCurr[i];
                var tmpGenes = genes[oldCurr];
                var len = tmpGenes.length;
                for(var r = 0;r<len;r++){
                    arrCurr[i] = tmpGenes[r];
                    var strCurr = arrCurr.join('');
                    if(bankSet.has(strCurr)){
                        bankSet.delete(strCurr);
                        queue.unshift(strCurr);
                    }
                }
                arrCurr[i] = oldCurr;
            }
            size--;
        }
        level++;
    }
    return -1;
};
```
+ 优化+2
  + 以上两种代码中 
  + queue队列的基因串分别进行重组时，转成了数组 
    + 在重组时临时修改了原基因串内的一个字符 并保留旧值
    + 在经过判断是否在基因库中后 又还原了原字符串 只是将临时重组且符合基因库的重组字符串加入队列而已
      + 其它语言如java或许需要这样做 但 javascript不需要
      + 在JS中，字符串即是只读数组，且拥有引用类型数组array的slice截取操作 
    + 所以重组字符串那块代码可以改成js特有的形式代码
  + level计算步数利用了内层while的size迭代操作直至size为0即当前queue队列所有字符都经过重组后且没有符合题意的条件时，level步数才自增+1
    + 可以直接将步数绑定在每一步每一轮组合中出队的字符串中一起
      + 每次出队重组后的字符串判断是否符合目标串 即更新它所携带的步数
      + 表明此轮操作要到达当前的出队的重组后的字符串自身所需步数
      + 找到符合的直接返回它携带的步数 
      + 即为所求
```javascript
/**
 * @param {string} start
 * @param {string} end
 * @param {string[]} bank
 * @return {number}
 */
var minMutation = function(start, end, bank) {
    if(start == end){
        return 0;
    }
    var bankSet = new Map();
    for(var i = 0;i<bank.length;i++){
        bankSet.set(bank[i],true);
    }
    var genes = ['A','C','G','T'];
    var level = 0;
    var queue = [[start,0]];
    while(queue.length != 0){
        var curr = queue.pop();
        level = curr[1]
        if(curr[0] == end){
            return level;
        }
        var arrCurr = curr[0];
        for(var i = 0;i<arrCurr.length;i++){
            for(var r = 0;r<genes.length;r++){
                if(genes[r] != arrCurr[i]){
                    var strCurr = (arrCurr.slice(0,i))+genes[r]+(arrCurr.slice(i+1));
                    if(bankSet.has(strCurr)){
                        queue.unshift([strCurr,level+1]);
                        bankSet.delete(strCurr);
                    }
                }
            }
        }
    }
    return -1;
};
```
#### 解法二：BFS-2
+ 图解
![截屏2019-11-07下午4.07.34.png](https://pic.leetcode-cn.com/d9efa9bebc77949e89e023ae6fdc8a55ead84ec08cb473550dd3315446d749a4-%E6%88%AA%E5%B1%8F2019-11-07%E4%B8%8B%E5%8D%884.07.34.png)
+ [思路同127. 单词接龙-解法一](https://leetcode-cn.com/problems/word-ladder/solution/127-dan-ci-jie-long-by-alexer-660/)
+ 注意不同的是步数的算法
  + 127题是第一变到下一个，当前值也算，即两步
  + 所以本题一端的搜索节点要从0步开始 
```javascript
/**
 * @param {string} start
 * @param {string} end
 * @param {string[]} bank
 * @return {number}
 */
var minMutation = function(start, end, bank) {
   if(!end || bank.indexOf(end) == -1){
        return -1;
    }
	// 各个通用状态对应所有单词
	var comboDicts = {};
	var len = start.length;
    for(var i = 0;i<bank.length;i++){
		for(var r = 0;r<len;r++){
			var newWord = bank[i].substring(0,r)+'*'+bank[i].substring(r+1,len);
			(!comboDicts[newWord]) && (comboDicts[newWord] = []);
			comboDicts[newWord].push(bank[i]);
		}
	}
	// Queue for BFS
	var queue = [[start,0]];
	// visited
	var visited = {start:true};
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
                    if(tmpWords[z] == end){
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
	return -1;
};
```
#### 解法三：BFS-3
+ 双端BFS
+ [思路同127. 单词接龙-解法三](https://leetcode-cn.com/problems/word-ladder/solution/127-dan-ci-jie-long-by-alexer-660/)
+ 注意不同的是步数的算法
  + 127题是第一变到下一个，当前值也算，即两步
  + 所以本题一端的搜索节点要从0步开始 
```javascript
/**
 * @param {string} start
 * @param {string} end
 * @param {string[]} bank
 * @return {number}
 */
var minMutation = function(start, end, bank) {
   if(!end || bank.indexOf(end) == -1){
        return -1;
    }
	// 各个通用状态对应所有单词
	var comboDicts = {};
	var len = start.length;
    for(var i = 0;i<bank.length;i++){
		for(var r = 0;r<len;r++){
			var newWord = bank[i].substring(0,r)+'*'+bank[i].substring(r+1,len);
			(!comboDicts[newWord]) && (comboDicts[newWord] = []);
			comboDicts[newWord].push(bank[i]);
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
	var queueBegin = [[start,1]];
    // Queue for BFS from endWord
    var queueEnd  = [[end,0]];
	// visited begin and end
	var visitedBegin = {};
    visitedBegin[start] = 1;
	var visitedEnd = {};
    visitedEnd[end] = 0;
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
	return -1;
};
```
#### 解法四：DFS
+ 深度优先搜索（从上到下）
  + 本题中 用于选择一种重组后的字符串则立即进行递归深入 直到符合题意 记录需要步数
  + 递归上一步
  + 结果即为 所有步数中最小的才符合题意
    + 循环中对于递归搜索过的重复字符串做一个visited 处理避免重复遍历
    + 因其结果不止一个 我们根据题意只取最少步数 所以维护一个变量 minLevel 每次更新
    + 每次只能重组一个字符 才可以作为下一步变换操作
      + 因此可以寻找与基因库组中只差一个字符的动态变换基因字符串（初始为起始基因字符串）
      + 找到则说明 此字符串可以作为其中一步变换的字符串 但不一定是最优解
      + 所以记录每一种组合下n轮所用的步数
  + 得到步数数组 取最小（通过每次取最小 只维护一个变量） 
    + 因为初始minLevel并不知道 所以通过设定一个js Number类型中可以表示但最大安全数作为初始值 每次比较取最小
    + 如果没有可以变换的步数可能题意要求返回-1 此时minLevel依旧是初始最大值 可以通过判断是否等于最大值来判断
      + 优化技巧：通过两个数的按位异或的位运算恒等于0 来判断是否应该返回-1 即minLevel初始值没变   
```javascript
/**
 * @param {string} start
 * @param {string} end
 * @param {string[]} bank
 * @return {number}
 */
var minMutation = function(start, end, bank) {
    if(start == end){
        return 0;
    }else if(!bank || bank.length == 0){
        return -1;
    }
    var visited = new Map();
    var minLevel = Number.MAX_SAFE_INTEGER;
    var level = 0;
    function recurse(start,level){
        if(start == end){
            minLevel = Math.min(minLevel,level);
        }
        for(var i = 0;i<bank.length;i++){
            var tmpBank = bank[i];
            var diff = 0;
            for(var r = 0;r<tmpBank.length;r++){
                if(start[r] != tmpBank[r]){
                    diff++;
                    if(diff > 1){
                        break;
                    }
                }
            }
            if(diff == 1 && !visited.has(tmpBank)){
                visited.set(tmpBank,true);
                recurse(tmpBank,level+1);
                visited.delete(tmpBank);
            }
        }
    }
   recurse(start,level);
   return (minLevel ^ Number.MAX_SAFE_INTEGER) == 0 ? -1 : minLevel;
};
```