#  N皇后（困难）
# 题目描述
![截屏2019-11-22下午2.58.19.png](https://pic.leetcode-cn.com/4f4468d2018990fff125b28f88628435f1e9b7505b7002bf2572cb1b11639ad0-%E6%88%AA%E5%B1%8F2019-11-22%E4%B8%8B%E5%8D%882.58.19.png)
![截屏2019-11-22下午2.58.28.png](https://pic.leetcode-cn.com/2506eb6a870bee2df48b33cf905c06fca5a526324d109143006abacf7c952fd7-%E6%88%AA%E5%B1%8F2019-11-22%E4%B8%8B%E5%8D%882.58.28.png)
# 题目地址
<https://leetcode-cn.com/problems/n-queens/>
#### 解法一：递归回溯
+ 思路
  + 从每一行开始，向上一行递归查询每一列是否符合皇后的摆放位置
    + 符合定义
      + 1、上n-1行同一列不能放
      + 2、上n-1行m-1列不能放
      + 3、上n-1行m+1列不能放
    + 2、3为对角线判断
    + 1为当前列
    + 每次都查上一行，当前行也自动规避了
```javascript
/**
 * @param {number} n
 * @return {string[][]}
 */
var solveNQueens = function(n) {
    let result = new Array(n);
    let results = [];
    // 回溯排除
    // 判断 row 行 column 列放置是否合适
    let dfs = (row,column) => {
        let leftColumn =  column-1;
        let rightColumn = column+1;
        // 逐上判断每一行是否能放棋子
        for(let i = row - 1;i >= 0;i--){
             // 第i行第column列是否有棋子【上一行同一列不能有棋子】
            if(result[i] == column){
                return false;
            }
            // 当左列存在 && 考虑左上对角线【递减斜向上】 第i行第leftColumn列是否有棋子
            if(leftColumn >= 0 && result[i] == leftColumn){
                return false;
            }
            // 当右列存在 && 考虑右上对角线【递减斜向上】 第i行第rightColumn列是否有棋子
            if(rightColumn < n && result[i] == rightColumn){
                return false;
            }
            leftColumn--;
            rightColumn++;
        }
        return true;
    }
    // 格式化数据格式
    let format = (result) => {
        let tmpResult = [];
        for(let i = 0;i < n;i++){
            let str = '';
            for(let j = 0;j < n;j++){
                if(result[i] == j){
                    str+='Q';
                }else{
                    str+='.';
                }
            }
            tmpResult.push(str);
        }
        return tmpResult;
    }
    // 开始搜索
    let Nqueens = (row) => {
        // 排除到最后一行，即当前所有行均排查过，
        // 意味着找到了一个解
        if(row == n){
            results.push(format(result));
            return;
        }
        for(let j = 0;j < n;j++){
            if(dfs(row,j)){
                result[row] = j;
                Nqueens(row+1)
            }
        }
    }
    // 从第一行开始搜索
    Nqueens(0);
    return results;
};
```
+ 优化版+1
  + format函数去掉，用数组的map函数，和字符串的repeat函数进行优化
  + 速度有提升
    ```javascript
    /**
     * @param {number} n
     * @return {string[][]}
     */
    var solveNQueens = function(n) {
        let result = new Array(n);
        let results = [];
        let dfs = (row,column) => {
            let leftColumn =  column-1;
            let rightColumn = column+1;
            for(let i = row - 1;i >= 0;i--){
                if(result[i] == column){
                    return false;
                }
                if(leftColumn >= 0 && result[i] == leftColumn){
                    return false;
                }
                if(rightColumn < n && result[i] == rightColumn){
                    return false;
                }
                leftColumn--;
                rightColumn++;
            }
            return true;
        }
        let Nqueens = (row) => {
            if(row == n){
                results.push(result.map(c=>'.'.repeat(c)+'Q'+'.'.repeat(n-1-c)));
                return;
            }
            for(let j = 0;j < n;j++){
                if(dfs(row,j)){
                    result[row] = j;
                    Nqueens(row+1)
                }
            }
        }
        Nqueens(0);
        return results;
    };
    ```
#### 解法二：对角线约束
+ 同一对角线上的两个坐标，它们的横坐标之差等于纵坐标之差
  + ![截屏2019-11-26下午2.16.10.png](https://pic.leetcode-cn.com/5da22c7121391469b8f77cb1fc4ec4e4b3df0123e665199a9dc6c518d5ed3597-%E6%88%AA%E5%B1%8F2019-11-26%E4%B8%8B%E5%8D%882.16.10.png)
```javascript
/**
 * @param {number} n
 * @return {string[][]}
 */
var solveNQueens = function (n) {
    const result = [];
    const dfs = ( subResult = [], row = 0) => {
        if (row === n) {
            result.push(subResult.map(c => '.'.repeat(c) + 'Q' + '.'.repeat(n - c - 1)));
            return;
        }
        for (let col = 0; col < n; col++) {
            if (!subResult.some((j, k) => j === col || j - col === row - k || j - col === k - row)) {
                subResult.push(col);
                dfs( subResult, row + 1);
                subResult.pop();
            }
        }
    }
    dfs();
    return result;
};
```