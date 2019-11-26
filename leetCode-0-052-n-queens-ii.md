# N皇后 II（困难）
# 题目描述
![截屏2019-11-26下午2.39.47.png](https://pic.leetcode-cn.com/e55a3b10b278f3a3c0f73d4b9a0cb12599ab7d128df6865138a54ad73f253f35-%E6%88%AA%E5%B1%8F2019-11-26%E4%B8%8B%E5%8D%882.39.47.png)
![截屏2019-11-26下午2.39.55.png](https://pic.leetcode-cn.com/368bf02509e4e97f9a8247a185a2aa55d8a172526d90c3814955ccfc8fa262de-%E6%88%AA%E5%B1%8F2019-11-26%E4%B8%8B%E5%8D%882.39.55.png)
# 题目地址
<https://leetcode-cn.com/problems/n-queens-ii/description/>
#### 解法一：递归回溯
+ [参考-51. N皇后-解法一](https://leetcode-cn.com/problems/n-queens/solution/51-nhuang-hou-by-alexer-660/)
  + ![截屏2019-11-26下午2.45.37.png](https://pic.leetcode-cn.com/b896844bf389d8e10438db680981fe4f2080c3846e639c25cd331b68a8447c47-%E6%88%AA%E5%B1%8F2019-11-26%E4%B8%8B%E5%8D%882.45.37.png)
```javascript
/**
 * @param {number} n
 * @return {number}
 */
var totalNQueens = function(n) {
    let result = new Array(n);
    let results = 0;
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
            results++;
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
+ [参考-51. N皇后-解法二](https://leetcode-cn.com/problems/n-queens/solution/51-nhuang-hou-by-alexer-660/)
```javascript
/**
 * @param {number} n
 * @return {number}
 */
var totalNQueens = function(n) {
    let result = 0;
    const dfs = ( subResult = [], row = 0) => {
        if (row === n) {
            result++;
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
#### 解法三：位运算
+ DFS
+ 当前列、左斜对角线、右斜对角线
+ 二进制为1代表不可放置，0相反
+ x & -x ：得到最低位的1
+ x & (x-1)：清零最低位的1
+ x & ((1 << n) - 1)：将x最高位至第n位(含)清零
```javascript
/**
 * @param {number} n
 * @return {number}
 */
var totalNQueens = function(n) {
    let res = 0;
    const dfs = (n,row,cols,pie,na) => {
        if (row >= n) {
            res++;
            return;
        }
        // 得到当前所有的空位
        let bits = (~(cols | pie | na)) & ((1 << n) -1)
        while(bits){
            // 取最低位的1
            let p = bits & -bits
            // 把P位置上放入皇后
            bits = bits & (bits - 1)
            dfs(n,row+1,cols | p,(pie | p) << 1,(na | p) >> 1)
        }
    }
    dfs(n,0,0,0,0);
    return res;
};
```