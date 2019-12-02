# 解数独（困难）
# 题目描述
![截屏2019-11-23上午10.47.52.png](https://pic.leetcode-cn.com/6b48364381c567f84217d5037adec293887abdb6c0c20bfe6f39398d3b6aed13-%E6%88%AA%E5%B1%8F2019-11-23%E4%B8%8A%E5%8D%8810.47.52.png)
![截屏2019-11-23上午10.48.01.png](https://pic.leetcode-cn.com/43b9b7b811eb24ab98fc8cb49135b17cbe6d0140f6850d2e199682db124b480f-%E6%88%AA%E5%B1%8F2019-11-23%E4%B8%8A%E5%8D%8810.48.01.png)
# 题目地址
<https://leetcode-cn.com/problems/sudoku-solver/>
#### 解法一：递归回溯
+ 类似题型
  + [36. 有效的数独](https://leetcode-cn.com/problems/valid-sudoku/solution/36-you-xiao-de-shu-du-by-alexer-660/)
+ 思路
  + 与36题区别的是：
    + 不止要判断填入的原本数字是否有效，且如果无效
      + 要重新回到第一次填的数字，重置重新填过，如1不行，填2，。。。填n（1=<n<=10）
      + 简而言之，数字是变动的，而且填错在下一步才发现的话之前的判重逻辑就要重置；也是36题我那对象存值的解法不适合再用的重要原因之一
  + 判重
    + 仅仅判断当前将要填入数字所在网格
      + 当前行其它列是否有重复的数字
      + 当前列其它行是否有重复的数字
      + 当前网格所处子数独是否有重复的数字
  + 回溯
    + 遍历1～9个数字，尝试将其中一个数字num放入空的格子内即 bord[i][j]== '.'时
    + 判重
      + 当前行、列、子数独有重复数字，换下一个数字
        + 如果遇到9个数字均无法填入空网格内而有重复的情况
          + 说明第一个空网格填入的数字有误，影响到了后面的填法
          + 此时回溯，回到第一种情况。尝试填如和前k次不同的数字，并重复判重填入动作
      + 无重复数字，填入当前网格内
        + 进入下一个空的网格重复上述动作
```javascript
/**
 * @param {character[][]} board
 * @return {void} Do not return anything, modify board in-place instead.
 */
var solveSudoku = function(board) {
    let isValid = (row,col,num) => {
        for(let i = 0;i < 9;i++){
            let boxRow = parseInt(row/3)*3;
            let boxCol = parseInt(col/3)*3;
            if(board[row][i] == num || board[i][col] == num || board[boxRow+parseInt(i/3)][boxCol+i%3] == num){
                return false;
            }
        }
        return true;
    }
    let solve = () => {
        for(let i = 0;i < 9;i++){
            for(let j = 0;j < 9;j++){
                if(board[i][j] == '.'){
                    for(let num = 1;num <10;num++){
                        if(isValid(i,j,num)){
                            board[i][j] = String(num);
                            if(solve(board)){
                                return true;
                            }
                            board[i][j] = '.';
                        }
                    }
                    return false;
                }
            }
        }
        return true;
    }
    solve(board);
    return board;
};
```
#### 解法二：递归回溯优化版
+ 待更