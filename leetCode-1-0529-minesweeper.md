# 扫雷游戏（中等）
# 题目描述
![截屏2019-12-14上午11.02.41.png](https://pic.leetcode-cn.com/1484539a87bf641a58851ec3b18fa1fece79a24900e6900adf1baa7cb0b3e5d4-%E6%88%AA%E5%B1%8F2019-12-14%E4%B8%8A%E5%8D%8811.02.41.png)
![截屏2019-12-14上午11.02.50.png](https://pic.leetcode-cn.com/aa65b97fafddd8af7fe1b24bd09023b0dc4465312a2e3246ec529c54f3910ba8-%E6%88%AA%E5%B1%8F2019-12-14%E4%B8%8A%E5%8D%8811.02.50.png)
![截屏2019-12-14上午11.03.00.png](https://pic.leetcode-cn.com/42968c82f4c1f5a7466e1f7d8337f13f083ae8f07a5d63eb732aec1fd90a7b87-%E6%88%AA%E5%B1%8F2019-12-14%E4%B8%8A%E5%8D%8811.03.00.png)
![截屏2019-12-14上午11.03.08.png](https://pic.leetcode-cn.com/73b1cd6d4951fc23473e72aaabd9f71a722661deccda472990a83eac833e7358-%E6%88%AA%E5%B1%8F2019-12-14%E4%B8%8A%E5%8D%8811.03.08.png)
# 题目地址
<https://leetcode-cn.com/problems/minesweeper/>
#### 解法一：DFS
+ 由题意
  + 'M' ：埋了一个地雷，没有挖出
  + 'X' ：埋了一个地雷，且已经挖出来了
    + 说明踩到地雷了，game over
  + 'E' ：埋了一个东西，可能是地雷，也可能是空的
  + 'B' ：一个已经挖出的空的东西，且它的四面八方没有地雷
    + 上、下、左、右、左上、左下、右上、右下
    + 对于每个四面八方没有地雷相邻的点作此深度优先搜索
      + 如果是非 'E'
        + 要么是未挖出的地雷 'M'
        + 要么是已经挖出的空白且四面八方没有地雷的方块
        + 要么是已经挖出地雷 'X'
      + 均退出循环，没有必要继续当前搜索，直接进入下一个方向的搜索
+ 求解
  + 如果当前点击位置是 'M' ，将它标记为 'X' ，并返回游戏板
  + 如果不是，说明点击的是 'E'
    + 计算当前点四面八方是否有地雷
      + 有，算出数量，并将当前点标记为此数量，结束当前方向的搜索
      + 无，将它标记为 'B'，并继续下一个方向的搜索
  + 搜索
    + 八大方向中的其中一点如果是 'M' 或者 'X'，说明有地雷
  + 边界
    + 当前点的横、纵坐标不能小于0，不能大于一维、二维长度
```javascript
/**
 * @param {character[][]} board
 * @param {number[]} click
 * @return {character[][]}
 */
var updateBoard = function(board, click) {
   let m = board.length;
   let n = board[0].length;
   let row = click[0];
   let col = click[1];
   if(board[row][col] == 'M'){
       board[row][col] = 'X';
   }else{
       let count = 0;
       for(let i = -1;i < 2;i++){
           for(let j = -1;j < 2;j++){
               if(i == 0 && j == 0){
                   continue;
               }
               let r = row + i;
               let c = col + j;
               if(r < 0 || r >= m || c < 0 || c >= n){
                   continue;
               }
               if(board[r][c] == 'M' || board[r][c] == 'X'){
                   count++;
               }
           }
       }
       if(count > 0){
           board[row][col] = count+'';
       }else{
           board[row][col] = 'B';
           for(let i = -1;i < 2;i++){
               for(let j = -1;j < 2;j++){
                    if(i == 0 && j == 0){
                        continue;
                    }
                    let r = row + i;
                    let c = col + j;
                    if(r < 0 || r >= m || c < 0 || c >= n){
                        continue;
                    }
                    if(board[r][c] == 'E'){
                        updateBoard(board,[r,c]);
                    }
               }
           }
       }
   }
    return board;
};
```
+ 或者这么写更好理解
```javascript
/**
 * @param {character[][]} board
 * @param {number[]} click
 * @return {character[][]}
 */
var updateBoard = function(board, click) {
   let x = click[0];
   let y = click[1];
   if(board[x][y] == 'M'){
       board[x][y] = 'X';
       return board;
   }
   let dx = [-1,-1,-1,1,1,1,0,0];
   let dy = [-1,1,0,-1,1,0,-1,1];
   let getNumsBombs = (board,x,y) => {
       let num = 0;
       for(let i = 0;i < 8;i++){
           let newX = x + dx[i];
           let newY = y + dy[i];
           if(newX < 0 || newX >= board.length || newY < 0 || newY >= board[0].length){
               continue;
           }
           if(board[newX][newY] == 'M' || board[newX][newY] == 'X'){
               num++;
           }
       }
       return num;
   }
   let dfs = (board,x,y) => {
       if(x < 0 || x >= board.length || y < 0 || y >= board[0].length || board[x][y] != 'E'){
           return;
       }
       let num = getNumsBombs(board,x,y);
       if(num == 0){
           board[x][y] = 'B';
           for(let i = 0;i < 8;i++){
               let newX = x + dx[i];
               let newY = y + dy[i];
               dfs(board,newX,newY);
           }
       }else{
           board[x][y] = num + '';
       }
   }
   dfs(board,x,y);
   return board;
};
```
#### 解法二：BFS
+ 同解法一一样，四面八方都要考虑，且刚好是相邻的节点，核心逻辑及写法一样
+ 区别是，用队列实现
```javascript
/**
 * @param {character[][]} board
 * @param {number[]} click
 * @return {character[][]}
 */
var updateBoard = function(board, click) {
  let m = board.length;
  let n = board[0].length;
  let queue = [click];
  while(queue.length != 0){
      let cell = queue.shift();
      let x = cell[0];
      let y = cell[1];
      if(board[x][y] == 'M'){
          board[x][y] = 'X';
      }else{
          let count = 0;
          for(let i = -1;i < 2;i++){
              for(let j = -1;j < 2;j++){
                  if(i == 0 && j == 0){
                      continue;
                  }
                  let newX = x + i;
                  let newY = y + j;
                  if(newX < 0 || newX >= m || newY < 0 || newY >= n){
                      continue;
                  }
                  if(board[newX][newY] == 'M' || board[newX][newY] == 'X'){
                      count++;
                  }
              }
          }
          if(count > 0){
              board[x][y] = count + '';
          }else{
              board[x][y] = 'B';
              for(let i = -1;i < 2;i++){
                  for(let j = -1;j < 2;j++){
                      if(i == 0 && j == 0){
                          continue;
                      }
                      let newX = x + i;
                      let newY = y + j;
                      if(newX < 0 || newX >= m || newY < 0 || newY >= n){
                          continue;
                      }
                      if(board[newX][newY] == 'E'){
                          queue.push([newX,newY]);
                          board[newX][newY] = 'B';
                      }
                  }
              }
          }
      }
  }
  return board;
}
```
+ 或者这样写
```javascript
/**
 * @param {character[][]} board
 * @param {number[]} click
 * @return {character[][]}
 */
var updateBoard = function(board, click) {
  if(board[click[0]][click[1]] == 'M'){
      board[click[0]][click[1]] = 'X';
      return board;
  }
  let m = board.length;
  let n = board[0].length;
  let dx = [1,1,1,-1,-1,-1,0,0];
  let dy = [-1,0,1,-1,0,1,-1,1];
  let queue = [click];
  let visited = {};
  visited[click[0]+'-'+click[1]] = true;
  let getNumsBombs = (board,x,y) => {
      let count = 0;
      for(let i = 0;i < 8;i++){
          let newX = x + dx[i];
          let newY = y + dy[i];
          if(newX == x && newY == y){
              continue;
          }
          if(newX >= 0 && newX < board.length && newY < board[0].length && newY >= 0){
              if(board[newX][newY] == 'M' || board[newX][newY] == 'X'){
                  count++;
              }
          }
      }
      return count;
  }
  while(queue.length != 0){
      let cell = queue.shift();
      let x = cell[0];
      let y = cell[1];
      let num = getNumsBombs(board,x,y);
      if(num == 0){
          board[x][y] = 'B';
          for(let i = 0;i < 8;i++){
              let newX = x + dx[i];
              let newY = y + dy[i];
              if(!visited[newX+'-'+newY] && newX >= 0 && newX < board.length && newY >= 0 && newY < board[0].length && board[newX][newY] == 'E'){
                  queue.push([newX,newY]);
                  visited[newX+'-'+newY] = true;
              }
          }
      }else{
          board[x][y] = num + '';
      }
  }
  return board;
}
```