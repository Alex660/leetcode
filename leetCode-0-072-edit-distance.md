# 编辑距离（困难）
# 题目描述
![截屏2019-11-17下午3.35.35.png](https://pic.leetcode-cn.com/a60adf99106408f83a27adfdebc47a9b2307b20a51fc2db1eed21f9b199712e1-%E6%88%AA%E5%B1%8F2019-11-17%E4%B8%8B%E5%8D%883.35.35.png)
# 题目地址
<https://leetcode-cn.com/problems/edit-distance/>
#### 解法一：递归回溯 
+ 思路
  + 如果word1[i]和word2[j]匹配
    + 则i++，j++继续比较下一个字符，无需替换、插入和删除操作
  + 如果word1[i]和word2[j]不匹配
    + 1、删除word1[i]
      + 然后递归考察word1[i+1]和word2[j]
    + 2、删除word2[j]
      + 然后递归考察word1[i]和word2[j+1]
    + 3、在word1[i]前面添加一个和word2[j]相同的字符
      + 然后递归考察word1[i]和word2[j+1]
    + 4、在word2[j]前面添加一个和word1[i]相同的字符
      + 然后递归考察word1[i+1]和word2[j]
    + 5、将word1[i]替换成word2[j]**或者**将word2[j]替换成word1[i]
      + 然后递归考察word1[i+1]和word2[j+1]
+ 结果：超时
  + 有很多重复子问题，也因此可以用更加高效的动态规划解决
```javascript
/**
 * @param {string} word1
 * @param {string} word2
 * @return {number}
 */
var minDistance = function(word1, word2) {
    var n = word1.length;
    var m = word2.length;
    var minDist = Number.MAX_SAFE_INTEGER;
    function helper(i,j,edist){
        // terminator
        if(i == n || j == m){
            // 没有剩余字符的单词要加上有剩余字符单词的剩余字符个数
            if(i < n){
                edist+=(n-i);
            }
            if(j < m){
                edist+=(m-j);
            }
            minDist = Math.min(edist,minDist);
            return;
        }
        // drill down
        // 如果两个子字符匹配即相同
        if(word1[i] == word2[j]){
            helper(i+1,j+1,edist);
        }else{
            // 删除a[i] 或 b[j]前添加一个字符
            helper(i+1,j,edist+1);
            // 删除b[j] 或 a[i]前添加一个字符
            helper(i,j+1,edist+1);
            // 将a[i]和b[j]替换为相同字符
            helper(i+1,j+1,edist+1);
        }
    }
    helper(0,0,0);
    return minDist;
};
```
#### 解法二：动态规划+自底向上-I
+ 图解
  + ![截屏2019-11-17下午11.18.45.png](https://pic.leetcode-cn.com/c7b5f3ae6b0a4a31849a10a93bb47675a94c5617cbdbbe25528e01ae4de2c6d7-%E6%88%AA%E5%B1%8F2019-11-17%E4%B8%8B%E5%8D%8811.18.45.png)
+ 在解法一的递归树中，(i,j)两个变量重复节点很多
  + 因此对于(i,j)相同的节点，只需要保留最小的edist即可
+ 由解法一的(i,j,edist)状态变成了(i,j,min_edist)
  + 状态(i,j)可以从以下三个状态中的任意一个转移过来
    + (i-1,j,min_edist)
    + (i,j-1,min_edist)
    + (i-1,j-1,min_edist)
  + 状态转移方程
    + 当word1[i] != word2[j]时
      + min_edist(i,j) = Min(min_edist(i-1,j)+1,min_edist(i,j-1)+1,min_edist(i-1,j-1)+1)
    + 当word1[i] == word2[j]时
      + min_edist(i,j) = Min(min_edist(i-1,j)+1,min_edist(i,j-1)+1,min_edist(i-1,j-1))
```javascript
/**
 * @param {string} word1
 * @param {string} word2
 * @return {number}
 */
var minDistance = function(word1, word2) {
    if(word1 == word2){
        return 0;
    }
    // 行
    var n = word1.length;
    // 列
    var m = word2.length;
    if(!n || !m){
        return n || m;
    }
    var minDist = Array.from(new Array(n),() => new Array(m));
    // 初始化第1列第n行的数据
    for(var i = 0;i < n;++i){
        if(word1[i] == word2[0]){
            minDist[i][0] = i;
        }else{
            if(i == 0){
                minDist[i][0] = 1;
            }else{
                minDist[i][0] = minDist[i-1][0] + 1;
            }
        }
    }
    // 初始化第1行第n列的数据
    for(var j = 0;j < m;++j){
        if(word1[0] == word2[j]){
            minDist[0][j] = j;
        }else{
            if(j == 0){
                minDist[0][j] = 1;
            }else{
                minDist[0][j] = minDist[0][j-1] + 1;
            }
        }
    }
    for(var i = 1;i < n;++i){
        for(var j = 1;j < m;++j){
            if(word1[i] == word2[j]){
                minDist[i][j] = Math.min(minDist[i-1][j]+1,minDist[i][j-1]+1,minDist[i-1][j-1]);
            }else{
                minDist[i][j] = Math.min(minDist[i-1][j]+1,minDist[i][j-1]+1,minDist[i-1][j-1]+1);
            }
        }
    }
    return minDist[n-1][m-1];
};
```
#### 解法三：动态规划+自底向上-II
+ 状态定义
  + dp[i][j]：代表word1到i位置要转换成word2到j位置所需要到最少步数
  + dp[i-1][j-1]：替换
  + dp[i-1][j]：删除或插入(添加)
  + dp[i][j-1]：删除或插入(添加)
+ 状态转移方程
  + 当word1[i] == word2[j]时
    + dp[i][j] = Min(dp[i][j-1],dp[i-1][j])+1 **或者**
    + dp[i][j] = dp[i-1][j-1]
  + 当word1[i] != word2[j]时
    + dp[i][j] = Min(dp[i][j-1],dp[i-1][j],dp[i-1][j-1])+1
+ 解题技巧
  + 第一种情况时，dp[i][j]也有两种情况
    + 可以把dp[i][j]的第一种情况合并到word1[i] != word2[j]中去
    + 第二种情况仅在word1[i-1] == word2[j-1]时出现
  + 因此递推公式可以精简为
    + 当word1[i-1] == word[j-1]时
      + dp[i][j] = dp[i-1][j-1]
    + 否则
      + dp[i][j] = Min(dp[i-1][j],dp[i][j-1],dp[i-1][j-1]) + 1
```javascript
/**
 * @param {string} word1
 * @param {string} word2
 * @return {number}
 */
var minDistance = function(word1, word2) {
    if(word1 == word2){
        return 0;
    }
    // 行
    var n = word1.length;
    // 列
    var m = word2.length;
    if(!n || !m){
        return n || m;
    }
    var dp = Array.from(new Array(n+1),() => new Array(m+1).fill(0));
    for(var i = 1;i <= n;++i){
        dp[i][0] = dp[i-1][0] + 1;
    }
    for(var j = 1;j <= m;++j){
        dp[0][j] = dp[0][j-1] + 1;
    }
    for(var i = 1;i <= n;++i){
        for(var j = 1;j <= m;++j){
            if(word1[i-1] == word2[j-1]){
                dp[i][j] = dp[i-1][j-1];
            }else{
                dp[i][j] = Math.min(dp[i-1][j],dp[i][j-1],dp[i-1][j-1])+1;
            }
        }
    }
    return dp[n][m];
};
```
#### 解法四：动态规划+自底向上-II+合并初始化
+ **''和非空单词的编辑距离是遍历时非空单词的索引值+1**
```javascript
/**
 * @param {string} word1
 * @param {string} word2
 * @return {number}
 */
var minDistance = function(word1, word2) {
    let n = word1.length;
    let m = word2.length;
    let dp = [];
    for(let i = 0;i <= n;i++){
        dp.push([])
        for(let j = 0;j <= m;j++){
            if(i*j){
                dp[i][j] = word1[i-1] == word2[j-1]? dp[i-1][j-1]: (Math.min(dp[i-1][j],dp[i][j-1],dp[i-1][j-1]) + 1);
            }else{
                dp[i][j] = i + j;
            }
        }
    }
    return dp[n][m];
};
```