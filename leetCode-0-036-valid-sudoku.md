# 有效的数独（中等）
# 题目描述
![截屏2019-11-23上午8.31.38.png](https://pic.leetcode-cn.com/4b18ff8b7bbcdea8d35baddbb3a515db46e3dd5c423fe62f15b482c3d13daa64-%E6%88%AA%E5%B1%8F2019-11-23%E4%B8%8A%E5%8D%888.31.38.png)
![截屏2019-11-23上午8.31.48.png](https://pic.leetcode-cn.com/0b63722588b2b3973e73097c0d276600a073d2e7409553f6575fc42b5748429b-%E6%88%AA%E5%B1%8F2019-11-23%E4%B8%8A%E5%8D%888.31.48.png)
![截屏2019-11-23上午8.32.03.png](https://pic.leetcode-cn.com/be32315c3705b0e857fad52327655adefd66742f83c7ce4d7a1332ce4b24ee9a-%E6%88%AA%E5%B1%8F2019-11-23%E4%B8%8A%E5%8D%888.32.03.png)
# 题目地址
<https://leetcode-cn.com/problems/valid-sudoku/submissions/>
#### 解法一：哈希判重
+ 行
  + 当前行9个数字不能有重复数字
+ 列
  + 当前列9个数字不能有重复数字
+ 九宫格
  + 当前子数独内没有重复数字
  + 9*9的数独划分为9个小的子数独
    + boxIndex = Math.floor(row/3) * 3 + Math.floor(columns/3)
    + 借用官方图
      + ![截屏2019-11-23上午8.38.08.png](https://pic.leetcode-cn.com/cd6292188dca8c7cf0ef92e5703d0a2326c736110c2be2277d96e78ba1ef90b8-%E6%88%AA%E5%B1%8F2019-11-23%E4%B8%8A%E5%8D%888.38.08.png)
```javascript
/**
 * @param {character[][]} board
 * @return {boolean}
 */
var isValidSudoku = function(board) {
    // 三个方向判重
    let rows = {};
    let columns = {};
    let boxes = {};
    // 遍历数独
    for(let i = 0;i < 9;i++){
        for(let j = 0;j < 9;j++){
            let num = board[i][j];
            if(num != '.'){
                // 子数独序号
                let boxIndex = parseInt((i/3)) * 3 + parseInt(j/3);
                if(rows[i+'-'+num] || columns[j+'-'+num] || boxes[boxIndex+'-'+num]){
                    return false;
                }
                // 以各自方向 + 不能出现重复的数字 组成唯一键值，若出现第二次，即为重复
                rows[i+'-'+num] = true;
                columns[j+'-'+num] = true;
                boxes[boxIndex+'-'+num] = true;
            }
        }
    }
    return true;
};
```
#### 解法二：位运算
+ 待更