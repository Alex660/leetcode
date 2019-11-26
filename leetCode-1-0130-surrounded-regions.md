# 被围绕的区域（中等）
# 题目描述
![截屏2019-11-22上午9.53.26.png](https://pic.leetcode-cn.com/5ae79f9fde64d69e57218f925b37ee107121eaa8159be43549ea1e1dbecfaab0-%E6%88%AA%E5%B1%8F2019-11-22%E4%B8%8A%E5%8D%889.53.26.png)
# 题目地址
<https://leetcode-cn.com/problems/surrounded-regions/>
#### 解法一：DFS 递归
+ 思路
  + 关键理解题意
    + 将与边界O不相连的内部O全部替换成X
  + 1、将边界O找出添加标识不能被替换
  + 2、通过DFS对边界O操作，将相连的O同样标识为不能被替换
  + 3、最后遍历，将非标识的O替换成题意X
+ 边界条件
  + 最上、下或最左、右，且是O的
+ 技巧
  + DFS中，已经被置为不能访问标识的无需再DFS
  + 最后遍历中，无需再遍历边界条件
```javascript
/**
 * @param {character[][]} board
 * @return {void} Do not return anything, modify board in-place instead.
 */
var solve = function(board) {
    let m = board.length;
    if(m == 0){return};
    let n = board[0].length;
    let cannot = {};
    let dfs = (i,j) => {
        // 越界、标示过或者非相连O下return
        if(i < 0 || j < 0 || i == m || j == n || board[i][j] != 'O' || cannot[i+'-'+j]){
            return;
        }
        cannot[i+'-'+j] = true;
        dfs(i-1,j);
        dfs(i+1,j);
        dfs(i,j-1);
        dfs(i,j+1);
    }
    for(let i = 0;i < m;i++){
        for(let j = 0;j < n;j++){
            // 从边缘O出发寻找其相连点都标示为不可替换
            if((i == 0 || j == 0 || i == m-1 || j == n-1) && board[i][j] == 'O'){
                dfs(i,j);
            }
        }
    }
    // 规避边界条件去循环
    for(let i = 1;i < m-1;i++){
        for(let j = 1;j < n-1;j++){
            if(!cannot[i+'-'+j] && board[i][j] == 'O'){
                board[i][j] = 'X';
            }
        }
    }
};
```
#### 解法二：DFS 非递归
+ 此种解法沿用解法一，用cannot来标识
+ 提供第二种，直接替换原数组元素来标识的手段
```javascript
/**
 * @param {character[][]} board
 * @return {void} Do not return anything, modify board in-place instead.
 */
var solve = function(board) {
    let m = board.length;
    if(m == 0){return};
    let n = board[0].length;
    let cannot = {};
    let dfs = (i,j) => {
        let stack = [];
        stack.push([i,j]);
        cannot[i+'-'+j] = true;
        while(stack.length > 0){
            let [I,J] = stack[stack.length-1];
            if(I - 1 >= 0 && !cannot[(I-1)+'-'+J] && board[I-1][J] == 'O'){
                stack.push([I-1,J]);
                cannot[(I-1)+'-'+J] = true;
                continue;
            }
            if(I + 1 < m && !cannot[(I+1)+'-'+J] && board[I+1][J] == 'O'){
                stack.push([I+1,J]);
                cannot[(I+1)+'-'+J] = true;
                continue;
            }
            if(J - 1 >= 0 && !cannot[I+'-'+(J-1)] && board[I][J-1] == 'O'){
                stack.push([I,J-1]);
                cannot[I+'-'+(J-1)] = true;
                continue;
            }
            if(J + 1 < n && !cannot[I+'-'+(J+1)] && board[I][J+1] == 'O'){
                stack.push([I,J+1]);
                cannot[I+'-'+(J+1)] = true;
                continue;
            }
            stack.pop();
        }
    }
    for(let i = 0;i < m;i++){
        for(let j = 0;j < n;j++){
            if((i == 0 || j == 0 || i == m-1 || j == n-1) && board[i][j] == 'O'){
                dfs(i,j);
            }
        }
    }
    for(let i = 0;i < m;i++){
        for(let j = 0;j < n;j++){
            if(!cannot[i+'-'+j] && board[i][j] == 'O'){
                board[i][j] = 'X';
            }
        }
    }
};
```
+ 或者这样写
```javascript
/**
 * @param {character[][]} board
 * @return {void} Do not return anything, modify board in-place instead.
 */
var solve = function(board) {
    let m = board.length;
    if(m == 0){return};
    let n = board[0].length;
    let dfs = (i,j) => {
        let stack = [];
        stack.push([i,j]);
        board[i][j] = '#';
        while(stack.length > 0){
            let [I,J] = stack[stack.length-1];
            if(I - 1 >= 0 && board[I-1][J] == 'O'){
                stack.push([I-1,J]);
                board[I-1][J] = '#';
                continue;
            }
            if(I + 1 < m && board[I+1][J] == 'O'){
                stack.push([I+1,J]);
                board[I+1][J] = '#';
                continue;
            }
            if(J - 1 >= 0 && board[I][J-1] == 'O'){
                stack.push([I,J-1]);
                board[I][J-1] = '#';
                continue;
            }
            if(J + 1 < n && board[I][J+1] == 'O'){
                stack.push([I,J+1]);
                board[I][J+1] = '#';
                continue;
            }
            stack.pop();
        }
    }
    for(let i = 0;i < m;i++){
        for(let j = 0;j < n;j++){
            if((i == 0 || j == 0 || i == m-1 || j == n-1) && board[i][j] == 'O'){
                dfs(i,j);
            }
        }
    }
    for(let i = 0;i < m;i++){
        for(let j = 0;j < n;j++){
            if(board[i][j] == 'O'){
                board[i][j] = 'X';
            }
            if(board[i][j] == '#'){
                board[i][j] = 'O';
            }
        }
    }
};
```
#### 解法三：BFS 非递归
+ 此种解法沿用解法一，用cannot来标识
+ 区别
  + dfs
    + 从上、下、左、右四个方向搜索，只要有一个搜索到了，就要continue，继续搜索它的上下左右入栈
  + bfs
    + 从上、下、左、右四个方向搜索，不管有没有搜索到，先把四个方向上符合的节点统一入队列
```javascript
/**
 * @param {character[][]} board
 * @return {void} Do not return anything, modify board in-place instead.
 */
var solve = function(board) {
    let m = board.length;
    if(m == 0){return};
    let n = board[0].length;
    let cannot = {};
    let queue = [];
    let bfs = () => {
        while(queue.length > 0){
            let [I,J] = queue.shift();
            if(!cannot[I+'-'+J]){
                cannot[I+'-'+J] = true;
                if(I - 1 >= 0 && board[I-1][J] == 'O'){
                    queue.push([I-1,J]);
                }
                if(I + 1 < m && board[I+1][J] == 'O'){
                    queue.push([I+1,J]);
                }
                if(J - 1 >= 0 && board[I][J-1] == 'O'){
                    queue.push([I,J-1]);
                }
                if(J + 1 < n && board[I][J+1] == 'O'){
                    queue.push([I,J+1]);
                }
            }
        }
    }
    for(let i = 0;i < m;i++){
        for(let j = 0;j < n;j++){
            if((i == 0 || j == 0 || i == m-1 || j == n-1) && board[i][j] == 'O'){
                queue.push([i,j]);
            }
        }
    }
    bfs();
    // 规避边界条件去循环
    for(let i = 1;i < m-1;i++){
        for(let j = 1;j < n-1;j++){
            if(!cannot[i+'-'+j] && board[i][j] == 'O'){
                board[i][j] = 'X';
            }
        }
    }
};
```
+ 或者这样写
```javascript
/**
 * @param {character[][]} board
 * @return {void} Do not return anything, modify board in-place instead.
 */
var solve = function(board) {
    let m = board.length;
    if(m == 0){return};
    let n = board[0].length;
    let queue = [];
    let bfs = () => {
        while(queue.length > 0){
            let [I,J] = queue.shift();
            board[I][J] = '#';
            if(I - 1 >= 0 && board[I-1][J] == 'O'){
                queue.push([I-1,J]);
            }
            if(I + 1 < m && board[I+1][J] == 'O'){
                queue.push([I+1,J]);
            }
            if(J - 1 >= 0 && board[I][J-1] == 'O'){
                queue.push([I,J-1]);
            }
            if(J + 1 < n && board[I][J+1] == 'O'){
                queue.push([I,J+1]);
            }
        }
    }
    for(let i = 0;i < m;i++){
        for(let j = 0;j < n;j++){
            if((i == 0 || j == 0 || i == m-1 || j == n-1) && board[i][j] == 'O'){
                queue.push([i,j]);
            }
        }
    }
    bfs();
    for(let i = 0;i < m;i++){
        for(let j = 0;j < n;j++){
            if(board[i][j] == 'O'){
                board[i][j] = 'X';
            }
            if(board[i][j] == '#'){
                board[i][j] = 'O';
            }
        }
    }
};
``` 
#### 解法四：并查集
+ [参考-547. 朋友圈-解法三、四](https://leetcode-cn.com/problems/friend-circles/solution/547-peng-you-quan-by-alexer-660/)
+ 分析
  + 正如547题所分析的那般
    + 并查集是一种树型的数据结构，用来**处理一些不相交集合(Disjoint Sets)的合并及查询问题**
  + 而此题解题关键由解法一分析可知
    + 将与边界O不相连的内部O全部替换成X 
  + 结合并查集这种数据结构
    + 我们可以将不符合题意的：与边界O相连的外部和内部O划分成A类
    + 将剩下的划分成B类
      + 包括与边界O不相连的内部O和所有X
    + 最后遍历整个数组
      + 判断不在A类且是O字符的全部替换成X，
      + 遍历结束后即为所求
  + 解题注意事项
    + 初始化
      + 对每一个元素进行初始化为单个代表或单类时
        + 通过组合i和j为唯一标识
          + 此题通过 i+'-'+j
          + 或者你可以通过 i * n + j 都可，一个道理
    + 如何划分成A类，根据并查集特点，得先有一个根节点
      + 与其从中选择一个，不如直接定义一个哨兵节点，不存在的根节点，属于A类的都挂靠在上面即可
    + 同样从边缘节点开始
      + 方便紧跟相连边缘O的内部O字符，找到就划分入A类，同解法一
    + 最后遍历只需要通过并查集算法中的Find方法
      + 它自带判断两个元素是否属于一个集合，即判断当前元素是否属于A类
    + 并查集定义、方法模板不清楚的[参考-547. 朋友圈-解法三、四分析](https://leetcode-cn.com/problems/friend-circles/solution/547-peng-you-quan-by-alexer-660/)
```javascript
/**
 * @param {character[][]} board
 * @return {void} Do not return anything, modify board in-place instead.
 */
var solve = function(board) {
    let m = board.length;
    if(m == 0){return};
    let n = board[0].length;
    let find = (p) => {
        while( p != parent[p]){
            parent[p] = parent[parent[p]];
            p = parent[p];
        }
        return p;
    }
    let union = (p,q) => {
        let rootP = find(p);
        let rootQ = find(q);
        if(rootP == rootQ){
            return;
        }
        parent[rootP] = rootQ;
    }
    // A类根节点
    let cannotSet = 'not';
    let parent = [];
    parent[cannotSet] = cannotSet;
    for(let i = 0;i < m;i++){
        for(let j = 0;j < n;j++){
            parent[i+'-'+j] = i+'-'+j;
        }
    }
    for(let i = 0;i < m;i++){
        for(let j = 0;j < n;j++){
            // 遇到O字符
            if(board[i][j] == 'O'){
                // 边缘O。不能替换，将其查并到A类下
                if(i == 0 || i == m-1 || j == 0 || j == n-1){
                    union(i+'-'+j,cannotSet);
                }
                // 紧跟边缘O的内部O，题意可知同样不能替换，将其查并到A类下
                else{
                    i -1 >= 0 && board[i-1][j] == 'O' && union(i+'-'+j,(i-1)+'-'+ j);
                    i + 1 < m && board[i+1][j] == 'O' && union(i+'-'+j,(i+1)+'-'+j) ;
                    j - 1 >= 0 && board[i][j-1] == 'O' && union(i+'-'+j,i +'-'+ (j - 1));
                    j + 1 < n &&  board[i][j+1] == 'O' && union(i+'-'+j,i +'-'+ (j + 1));
                }
            }
        }
    }
    // 从非边缘下开始遍历替换不属于A类下的O字符为X，完毕即为所求
    for(let i = 1;i < m-1;i++){
        for(let j = 1;j < n-1;j++){
            if( board[i][j] == 'O' && find(i +'-'+j) != find(cannotSet)){
                board[i][j] = 'X';
            }
        }
    }
};
```