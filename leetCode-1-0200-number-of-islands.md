# 岛屿数量（中等）
# 题目描述
![截屏2019-10-29下午7.53.52.png](https://pic.leetcode-cn.com/9b60ac53004a1a21721f8572db42a27def6c692d002e824f74f78a9fe0081f52-%E6%88%AA%E5%B1%8F2019-10-29%E4%B8%8B%E5%8D%887.53.52.png)
# 题目地址
<https://leetcode-cn.com/problems/number-of-islands/>
#### 解法一：DFS
```javascript
/**
 * @param {character[][]} grid
 * @return {number}
 */
var numIslands = function(grid) {
    if(!grid || grid.length == 0){
        return 0;
    }
    var len = grid.length;
    var size = grid[0].length;
    var island = 0;
    function sink(i,j){
        // terminator
        if(grid[i][j] == '0'){
            return 0;
        }
        // process
        grid[i][j] = '0';
        // drill down
        if(i+1<len && grid[i+1][j] == '1'){
            sink(i+1,j);
        } 
        if(i-1 >= 0 && grid[i-1][j] == '1'){
            sink(i-1,j);
        } 
        if(j+1 < size && grid[i][j+1] == '1'){
            sink(i,j+1);
        } 
        if(j-1 >= 0 && grid[i][j-1] == '1'){
            sink(i,j-1);
        }
        return 1;
    }
    for(var i = 0;i<len;i++){
        for(var r = 0;r<grid[i].length;r++){
            if(grid[i][r] == '1'){
                island += sink(i,r);
            }
        }
    }
    return island;
};
```
+ 优化版 
  + 判断并推平岛屿函数时 用方向向量替代x+1,x-1,y+1,y-1四种情况
```javascript
/**
 * @param {character[][]} grid
 * @return {number}
 */
var numIslands = function(grid) {
    if(!grid || grid.length == 0){
        return 0;
    }
    var len = grid.length;
    var size = grid[0].length;
    var island = 0;
    // 方向向量
    var dx = [-1,1,0,0];
    var dy = [0,0,-1,1];
    // dfs 推平
    function sink(i,j){
        // terminator
        if(grid[i][j] == '0'){
            return 0;
        }
        // process
        grid[i][j] = '0';
        // drill down
        for(var k = 0;k< dx.length;k++){
            var x = i + dx[k];
            var y = j + dy[k];
            if(x >= 0 && x < grid.length && y >=0 && y<grid[i].length){
                if(grid[x][y] == '1'){
                     sink(x,y)
                }
            }
        }
        return 1;
    }
    for(var i = 0;i<len;i++){
        for(var r = 0;r<grid[i].length;r++){
            if(grid[i][r] == 1){
                island += sink(i,r);
            }
        }
    }
    return island;
};
```
#### 解法二：BFS
```javascript
/**
 * @param {character[][]} grid
 * @return {number}
 */
var numIslands = function(grid) {
    if(!grid || grid.length == 0){
        return 0;
    }
    var len = grid.length;
    var size = grid[0].length;
    var island = 0;
    // 从右到左 队列
    var queue = [];
    // 方向向量
    var dx = [-1,1,0,0];
    var dy = [0,0,-1,1];
    // dfs 推平
    function sink(i,j){
        // terminator
        if(grid[i][j] == '0'){
            return 0;
        }
        // process
        grid[i][j] = '0';
        // drill down
        for(var k = 0;k< dx.length;k++){
            var x = i + dx[k];
            var y = j + dy[k];
            if(x >= 0 && x < grid.length && y >=0 && y<grid[i].length){
                if(grid[x][y] == '1'){
                     queue.push([x,y]);
                }
            }
        }
        return 1;
    }
    for(var i = 0;i<len;i++){
        for(var r = 0;r<grid[i].length;r++){
            if(grid[i][r] == 1){
                island++;
                queue.push([i,r])
                while(queue.length>0){
                    var tmpIsland = queue.shift();
                    sink(tmpIsland[0],tmpIsland[1]);
                }
            }
        }
    }
    return island;
};
```
#### 解法三：并查集
+ 类似题型
  + [547. 朋友圈](https://leetcode-cn.com/problems/friend-circles/solution/547-peng-you-quan-by-alexer-660/)
+ 区别
  + 此题是m * n的矩阵，不是n * n的矩阵
    + parent和rank数组元素命名要将i和j联系起来
    + union查找合并的时候，也要把两个是陆地的元素传进去
```javascript
/**
 * @param {character[][]} grid
 * @return {number}
 */
var numIslands = function(grid) {
    let m = grid.length;
    if(m == 0){
        return 0;
    }
    let n = grid[0].length;
    let count = 0;
    let parent = [];
    let rank = [];


    let find = (p) => {
        while(p != parent[p]){
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
        if(rank[rootP] > rank[rootQ]){
            parent[rootQ] = rootP;
        }else if(rank[rootP] < rank[rootQ]){
            parent[rootP] = rootQ;
        }else{
            parent[rootP] = rootQ;
            rank[rootQ]++;
        }
        count--;
    }

    for(let i = 0;i < m;i++){
        for(let j = 0;j < n;j++){
            if(grid[i][j] == 1){
                parent[i * n + j] = i * n + j;
                count++;
            }
            rank[i * n + j] = 0;
        }
    }

    for(var i = 0;i<m;i++){
        for(var j = 0;j<n;j++){
            if(grid[i][j] == 1){
                grid[i][j] = 0;
                i-1>=0 && grid[i-1][j] == 1 && union(i*n + j,(i-1)*n + j);
                j-1>=0 && grid[i][j-1] == 1 && union(i*n + j,i*n + j-1);
                i+1<m && grid[i+1][j] == 1 && union(i*n + j,(i+1)*n + j);
                j+1<n && grid[i][j+1] == 1 && union(i*n + j,i*n + j+1);
            }
        }
    }
    return count;
};
```